---
title: 逻辑解码 - Azure Database for PostgreSQL - 单一服务器
description: 介绍用于 Azure Database for PostgreSQL - 单一服务器中的变更数据捕获的逻辑解码和 wal2json
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 03/31/2020
ms.date: 04/27/2020
ms.openlocfilehash: 243aa831a1e5b257a1bf55b5033bd9a1d1923756
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127261"
---
# <a name="logical-decoding"></a>逻辑解码
 
使用 [PostgreSQL 中的逻辑解码](https://www.postgresql.org/docs/current/logicaldecoding.html)可将数据更改流式传输到外部使用者。 逻辑解码广泛用于事件流和变更数据捕获方案。

逻辑解码使用一个输出插件将 Postgres 的预写日志 (WAL) 转换为可读格式。 Azure Database for PostgreSQL 提供两个输出插件：[test_decoding](https://www.postgresql.org/docs/current/test-decoding.html) 和 [wal2json](https://github.com/eulerto/wal2json)。
 

> [!NOTE]
> Azure Database for PostgreSQL - 单一服务器上的逻辑解码目前为公共预览版。


## <a name="set-up-your-server"></a>设置服务器
若要开始使用逻辑解码，请让服务器保存并流式传输 WAL。 

1. 使用 Azure CLI 将 azure.replication_support 设置为 `logical`。 
   ```
   az postgres server configuration set --resource-group mygroup --server-name myserver --name azure.replication_support --value logical
   ```

   > [!NOTE]
   > 如果使用只读副本，则将 azure.replication_support 设置为 `logical` 还可允许副本运行。 如果停止使用逻辑解码，请将该设置改回到 `replica`。 


2. 重启服务器以应用更改。
   ```
   az postgres server restart --resource-group mygroup --name myserver
   ```

## <a name="start-logical-decoding"></a>开始逻辑解码

可以通过流式处理协议或 SQL 接口使用逻辑解码。 这两种方法都使用[复制槽](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS)。 槽表示来自单一数据库的更改流。

使用复制槽需要 Postgres 的复制特权。 目前，复制特权仅适用于服务器的管理员用户。 

### <a name="streaming-protocol"></a>流式处理协议
我们通常倾向于通过流式处理协议使用更改。 你可以创建自己的使用者/连接器，或者使用 [Debezium](https://debezium.io/) 之类的工具。 

请访问 wal2json 文档，查看[将流式处理协议与 pg_recvlogical 配合使用的示例](https://github.com/eulerto/wal2json#pg_recvlogical)。


### <a name="sql-interface"></a>SQL 接口
以下示例将 SQL 接口与 wal2json 插件配合使用。
 
1. 创建槽。
   ```SQL
   SELECT * FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
   ```
 
2. 发出 SQL 命令。 例如：
   ```SQL
   CREATE TABLE a_table (
      id varchar(40) NOT NULL,
      item varchar(40),
      PRIMARY KEY (id)
   );
   
   INSERT INTO a_table (id, item) VALUES ('id1', 'item1');
   DELETE FROM a_table WHERE id='id1';
   ```

3. 使用更改。
   ```SQL
   SELECT data FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', '1');
   ```

   输出如下所示：
   ```
   {
         "change": [
         ]
   }
   {
         "change": [
                  {
                           "kind": "insert",
                           "schema": "public",
                           "table": "a_table",
                           "columnnames": ["id", "item"],
                           "columntypes": ["character varying(40)", "character varying(40)"],
                           "columnvalues": ["id1", "item1"]
                  }
         ]
   }
   {
         "change": [
                  {
                           "kind": "delete",
                           "schema": "public",
                           "table": "a_table",
                           "oldkeys": {
                                 "keynames": ["id"],
                                 "keytypes": ["character varying(40)"],
                                 "keyvalues": ["id1"]
                           }
                  }
         ]
   }
   ```

4. 用完槽后，请将其删除。
   ```SQL
   SELECT pg_drop_replication_slot('test_slot'); 
   ```


## <a name="monitoring-slots"></a>监视槽

必须监视逻辑解码。 必须删除任何未使用的复制槽。 在使用者读取更改之前，槽会一直保存到 Postgres WAL 日志和相关的系统目录。 如果使用者失败或者未经正确配置，则未使用的日志将不断堆积，直至填满存储。 此外，未使用的日志会增大事务 ID 换行的风险。 这两种情况可能会导致服务器不可用。 因此，逻辑复制槽被持续使用是至关重要的。 如果不再使用某个逻辑复制槽，请立即将其删除。

pg_replication_slots 视图中的“active”列指示是否有使用者连接到某个槽。
```SQL
SELECT * FROM pg_replication_slots;
```

请针对“已用存储”和“副本的最大滞后时间”指标[设置警报](howto-alert-on-metric.md)，以便在值超过正常阈值时收到通知。   

> [!IMPORTANT]
> 必须删除未使用的复制槽。 否则可能会导致服务器不可用。

## <a name="how-to-drop-a-slot"></a>如何删除槽
如果你未积极使用某个复制槽，应将其删除。

若要使用 SQL 删除名为 `test_slot` 的复制槽：
```SQL
SELECT pg_drop_replication_slot('test_slot');
```

> [!IMPORTANT]
> 如果停止使用逻辑解码，请将 azure.replication_support 改回到 `replica` 或 `off`。 通过 `logical` 保留的 WAL 详细信息更详尽，应在逻辑解码未在使用时被禁用。 

 
## <a name="next-steps"></a>后续步骤

* 访问 Postgres 文档来[详细了解逻辑解码](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html)。
* 如果在逻辑解码方面有任何问题，请联系[我们的团队](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)。
* 详细了解[只读副本](concepts-read-replicas.md)。

