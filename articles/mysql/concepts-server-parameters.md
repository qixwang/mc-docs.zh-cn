---
title: 服务器参数 - Azure Database for MySQL
description: 本主题提供了在 Azure Database for MySQL 中配置服务器参数的准则。
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: conceptual
origin.date: 6/5/2020
ms.date: 06/29/2020
ms.openlocfilehash: 52a2a6a66bfe86b93a441d70cbe7b08406da7d9d
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516848"
---
# <a name="server-parameters-in-azure-database-for-mysql"></a>Azure Database for MySQL 中的服务器参数

本文提供了在 Azure Database for MySQL 中配置服务器参数的注意事项和准则。

## <a name="what-are-server-parameters"></a>什么是服务器参数？ 

MySQL 引擎提供了可以用来配置和优化引擎行为的许多不同的服务器变量/参数。 某些参数可在运行时动态设置，另外一些参数则为“静态”参数，需要重启服务器才能应用。

Azure Database for MySQL 公开了通过 [Azure 门户](./howto-server-parameters.md)、[Azure CLI](./howto-configure-server-parameters-using-cli.md) 和 [PowerShell](./howto-configure-server-parameters-using-powershell.md) 根据工作负荷需求更改各种 MySQL 服务器参数值的功能。

## <a name="configurable-server-parameters"></a>可配置的服务器参数

受支持服务器参数的列表还在不断增加。 在 Azure 门户中使用服务器参数选项卡可查看完整列表并配置服务器参数值。

请参阅以下各部分，详细了解多个经常更新的服务器参数的限制。 这些限制取决于服务器的定价层和 vCore 数。

### <a name="innodb_buffer_pool_size"></a>innodb_buffer_pool_size

查看 [MySQL 文档](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size)详细了解此参数。

#### <a name="servers-supporting-up-to-4-tb-storage"></a>最多支持 4 TB 存储的服务器

|**定价层**|**vCore(s)**|**默认值（字节）**|**最小值（字节）**|**最大值（字节）**|
|---|---|---|---|---|
|基本|1|872415232|134217728|872415232|
|基本|2|2684354560|134217728|2684354560|
|常规用途|2|3758096384|134217728|3758096384|
|常规用途|4|8053063680|134217728|8053063680|
|常规用途|8|16106127360|134217728|16106127360|
|常规用途|16|32749125632|134217728|32749125632|
|常规用途|32|66035122176|134217728|66035122176|
|常规用途|64|132070244352|134217728|132070244352|
|内存优化|2|7516192768|134217728|7516192768|
|内存优化|4|16106127360|134217728|16106127360|
|内存优化|8|32212254720|134217728|32212254720|
|内存优化|16|65498251264|134217728|65498251264|
|内存优化|32|132070244352|134217728|132070244352|

### <a name="innodb_file_per_table"></a>innodb_file_per_table

> [!NOTE]
> `innodb_file_per_table` 只能在“常规用途”和“内存优化”定价层中更新。

MySQL 根据你在表创建期间提供的配置，将 InnoDB 表存储在不同的表空间中。 [系统表空间](https://dev.mysql.com/doc/refman/5.7/en/innodb-system-tablespace.html)是 InnoDB 数据字典的存储区域。 [file-per-table 表空间](https://dev.mysql.com/doc/refman/5.7/en/innodb-file-per-table-tablespaces.html)包含单个 InnoDB 表的数据和索引，并存储在文件系统内它自己的数据文件中。 此行为由 `innodb_file_per_table` 服务器参数控制。 将 `innodb_file_per_table` 设置为 `OFF` 会导致 InnoDB 在系统表空间中创建表。 否则，InnoDB 在 file-per-table 表空间中创建表。

在单个数据文件中，Azure Database for MySQL 支持最大 1TB。 如果数据库大小超过 1TB，应在 [innodb_file_per_table](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_file_per_table) 表空间中创建表。 如果单个表的大小超过 1 TB，应使用分区表。

### <a name="join_buffer_size"></a>join_buffer_size

查看 [MySQL 文档](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_join_buffer_size)详细了解此参数。

|**定价层**|**vCore(s)**|**默认值（字节）**|**最小值（字节）**|**最大值（字节）**|
|---|---|---|---|---|
|基本|1|在基本层中不可配置|空值|空值|
|基本|2|在基本层中不可配置|空值|空值|
|常规用途|2|262144|128|268435455|
|常规用途|4|262144|128|536870912|
|常规用途|8|262144|128|1073741824|
|常规用途|16|262144|128|2147483648|
|常规用途|32|262144|128|4294967295|
|常规用途|64|262144|128|4294967295|
|内存优化|2|262144|128|536870912|
|内存优化|4|262144|128|1073741824|
|内存优化|8|262144|128|2147483648|
|内存优化|16|262144|128|4294967295|
|内存优化|32|262144|128|4294967295|

### <a name="max_connections"></a>max_connections

|**定价层**|**vCore(s)**|**默认值**|**最小值**|**最大值**|
|---|---|---|---|---|
|基本|1|50|10 个|50|
|基本|2|100|10 个|100|
|常规用途|2|300|10 个|600|
|常规用途|4|625|10 个|1250|
|常规用途|8|1250|10 个|2500|
|常规用途|16|2500|10 个|5000|
|常规用途|32|5000|10 个|10000|
|常规用途|64|10000|10 个|20000|
|内存优化|2|625|10 个|1250|
|内存优化|4|1250|10 个|2500|
|内存优化|8|2500|10 个|5000|
|内存优化|16|5000|10 个|10000|
|内存优化|32|10000|10 个|20000|

当连接数超出限制时，可能会收到以下错误：
> 错误 1040 (08004)：连接过多

> [!IMPORTANT]
> 为了获得最佳体验，我们建议你使用 ProxySQL 之类的连接池程序来有效地管理连接。

创建与 MySQL 的新客户端连接需要时间，一旦建立，这些连接就会占用数据库资源，即使在空闲时也是如此。 大多数应用程序请求许多生存期短的连接，这加剧了这种情况。 其结果是可用于实际工作负荷的资源减少，从而导致性能下降。 连接池程序不仅会减少空闲连接，还会重用现有连接，因而有助于避免这种情况。 若要了解如何设置 ProxySQL，请访问我们的[博客文章](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042)。

### <a name="max_heap_table_size"></a>max_heap_table_size

查看 [MySQL 文档](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_max_heap_table_size)详细了解此参数。

|**定价层**|**vCore(s)**|**默认值（字节）**|**最小值（字节）**|**最大值（字节）**|
|---|---|---|---|---|
|基本|1|在基本层中不可配置|空值|空值|
|基本|2|在基本层中不可配置|空值|空值|
|常规用途|2|16777216|16384|268435455|
|常规用途|4|16777216|16384|536870912|
|常规用途|8|16777216|16384|1073741824|
|常规用途|16|16777216|16384|2147483648|
|常规用途|32|16777216|16384|4294967295|
|常规用途|64|16777216|16384|4294967295|
|内存优化|2|16777216|16384|536870912|
|内存优化|4|16777216|16384|1073741824|
|内存优化|8|16777216|16384|2147483648|
|内存优化|16|16777216|16384|4294967295|
|内存优化|32|16777216|16384|4294967295|

### <a name="query_cache_size"></a>query_cache_size

默认会禁用查询缓存。 若要启用查询缓存，请配置 `query_cache_type` 参数。 

查看 [MySQL 文档](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_query_cache_size)详细了解此参数。

> [!NOTE]
> 查询缓存从 MySQL 5.7.20 开始已遭弃用，并且已在 MySQL 8.0 中删除

|**定价层**|**vCore(s)**|**默认值（字节）**|**最小值（字节）**|**最大值 **|
|---|---|---|---|---|
|基本|1|在基本层中不可配置|空值|空值|
|基本|2|在基本层中不可配置|空值|空值|
|常规用途|2|0|0|16777216|
|常规用途|4|0|0|33554432|
|常规用途|8|0|0|67108864|
|常规用途|16|0|0|134217728|
|常规用途|32|0|0|134217728|
|常规用途|64|0|0|134217728|
|内存优化|2|0|0|33554432|
|内存优化|4|0|0|67108864|
|内存优化|8|0|0|134217728|
|内存优化|16|0|0|134217728|
|内存优化|32|0|0|134217728|

### <a name="sort_buffer_size"></a>sort_buffer_size

查看 [MySQL 文档](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_sort_buffer_size)详细了解此参数。

|**定价层**|**vCore(s)**|**默认值（字节）**|**最小值（字节）**|**最大值（字节）**|
|---|---|---|---|---|
|基本|1|在基本层中不可配置|空值|空值|
|基本|2|在基本层中不可配置|空值|空值|
|常规用途|2|524288|32768|4194304|
|常规用途|4|524288|32768|8388608|
|常规用途|8|524288|32768|16777216|
|常规用途|16|524288|32768|33554432|
|常规用途|32|524288|32768|33554432|
|常规用途|64|524288|32768|33554432|
|内存优化|2|524288|32768|8388608|
|内存优化|4|524288|32768|16777216|
|内存优化|8|524288|32768|33554432|
|内存优化|16|524288|32768|33554432|
|内存优化|32|524288|32768|33554432|

### <a name="tmp_table_size"></a>tmp_table_size

查看 [MySQL 文档](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_tmp_table_size)详细了解此参数。

|**定价层**|**vCore(s)**|**默认值（字节）**|**最小值（字节）**|**最大值（字节）**|
|---|---|---|---|---|
|基本|1|在基本层中不可配置|空值|空值|
|基本|2|在基本层中不可配置|空值|空值|
|常规用途|2|16777216|1024|67108864|
|常规用途|4|16777216|1024|134217728|
|常规用途|8|16777216|1024|268435456|
|常规用途|16|16777216|1024|536870912|
|常规用途|32|16777216|1024|1073741824|
|常规用途|64|16777216|1024|1073741824|
|内存优化|2|16777216|1024|134217728|
|内存优化|4|16777216|1024|268435456|
|内存优化|8|16777216|1024|536870912|
|内存优化|16|16777216|1024|1073741824|
|内存优化|32|16777216|1024|1073741824|

### <a name="time_zone"></a>time_zone

可以通过从 MySQL 命令行或 MySQL Workbench 等工具调用 `mysql.az_load_timezone` 存储过程来填充时区表。 若要了解如何调用存储过程并设置全局时区或会话级时区，请参阅 [Azure 门户](howto-server-parameters.md#working-with-the-time-zone-parameter)或 [Azure CLI](howto-configure-server-parameters-using-cli.md#working-with-the-time-zone-parameter) 一文。

## <a name="non-configurable-server-parameters"></a>不可配置的服务器参数

以下服务器参数不可在服务中配置：

|**参数**|**固定值**|
| :------------------------ | :-------- |
|基本层中的 innodb_file_per_table|OFF|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|256 MB|
|innodb_log_files_in_group|2|

此处未列出的其他变量将设置为默认的 MySQL 现成值。 有关默认值，请参阅适用于版本 [8.0](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html)、[5.7](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html) 和 [5.6](https://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html) 的 MySQL 文档。 

## <a name="next-steps"></a>后续步骤

- 了解如何[使用 Azure 门户配置服务器参数](./howto-server-parameters.md)
- 了解如何[使用 Azure CLI 配置服务器参数](./howto-configure-server-parameters-using-cli.md)
- 了解如何[使用 PowerShell 配置服务器参数](./howto-configure-server-parameters-using-powershell.md)