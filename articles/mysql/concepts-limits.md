---
title: 限制 - Azure Database for MySQL
description: 本文介绍了 Azure Database for MySQL 中的限制，例如连接数和存储引擎选项。
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: conceptual
origin.date: 12/9/2019
ms.date: 03/02/2020
ms.openlocfilehash: 18b0b2a72b888d9a777a6f03fb6253be941157de
ms.sourcegitcommit: 892137d117bcaf9d88aec0eb7ca756fe39613344
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78154350"
---
# <a name="limitations-in-azure-database-for-mysql"></a>Azure Database for MySQL 中的限制

> [!NOTE]
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql-database-on-azure/)。

以下各部分介绍了数据库服务中的容量、存储引擎支持、特权支持、数据操作语句支持和功能限制。 另请参阅适用于 MySQL 数据库引擎的[常规限制](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html)。

## <a name="maximum-connections"></a>最大连接数
每个定价层的最大连接数和 vCore 数如下所示： 

|**定价层**|**vCore(s)**| 最大连接数 |
|---|---|---|
|基本| 1| 50|
|基本| 2| 100|
|常规用途| 2| 600|
|常规用途| 4| 1250|
|常规用途| 8| 2500|
|常规用途| 16| 5000|
|常规用途| 32| 10000|
|常规用途| 64| 20000|
|内存优化| 2| 1250|
|内存优化| 4| 2500|
|内存优化| 8| 5000|
|内存优化| 16| 10000|
|内存优化| 32| 20000|

当连接数超出限制时，可能会收到以下错误：
> 错误 1040 (08004)：连接过多

> [!IMPORTANT]
> 为了获得最佳体验，我们建议你使用 ProxySQL 之类的连接池程序来有效地管理连接。

创建与 MySQL 的新客户端连接需要时间，一旦建立，这些连接就会占用数据库资源，即使在空闲时也是如此。 大多数应用程序都请求许多短期连接，这加剧了这种情况。 其结果是可用于实际工作负荷的资源减少，从而导致性能下降。 减少空闲连接并重用现有连接的连接池会有助于避免这种情况。 若要了解如何设置 ProxySQL，请访问我们的[博客文章](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042)。

## <a name="storage-engine-support"></a>存储引擎支持

### <a name="supported"></a>支持
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMORY](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>不支持
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIVE](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERATED](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>特权支持

### <a name="unsupported"></a>不支持
- DBA 角色：许多服务器参数和设置可能会无意中导致服务器性能下降或使 DBMS 的 ACID 属性无效。 因此，为了维护产品级别的服务完整性和 SLA，此服务不公开 DBA 角色。 默认用户帐户（在创建新的数据库实例时构造）允许该用户执行托管数据库实例中的大部分 DDL 和 DML 语句。 
- SUPER 特权：[SUPER 特权](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super)同样也受到限制。
- DEFINER：需要创建并限制超级权限。 如果使用备份导入数据，请在执行 mysqldump 时手动删除或使用 `--skip-definer` 命令删除 `CREATE DEFINER` 命令。

## <a name="data-manipulation-statement-support"></a>数据操作语句支持

### <a name="supported"></a>支持
- 支持 `LOAD DATA INFILE`，但必须指定 `[LOCAL]` 参数，并将其定向到 UNC 路径（通过 SMB 装载的 Azure 存储空间）。

### <a name="unsupported"></a>不支持
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>功能限制

### <a name="scale-operations"></a>缩放操作
- 目前不支持动态缩放到“基本”定价层或从该层动态缩放。
- 不支持减小服务器存储大小。

### <a name="server-version-upgrades"></a>服务器版本升级
- 目前不支持在主要数据库引擎版本之间进行自动迁移。 如果要升级到下一个主版本，请进行[转储并将其还原](./concepts-migrate-dump-restore.md)到使用新引擎版本创建的服务器。

### <a name="point-in-time-restore"></a>时间点还原
- 使用 PITR 功能时，将使用与新服务器所基于的服务器相同的配置创建新服务器。
- 不支持还原已删除的服务器。

### <a name="vnet-service-endpoints"></a>VNet 服务终结点
- 只有常规用途和内存优化服务器才支持 VNet 服务终结点。

### <a name="storage-size"></a>存储大小
- 有关每个定价层的存储大小限制，请参阅[定价层](concepts-pricing-tiers.md)。

## <a name="current-known-issues"></a>当前已知的问题
- 建立连接后，MySQL 服务器实例显示错误的服务器版本。 若要获取正确的服务器实例引擎版本，请使用 `select version();` 命令。

## <a name="next-steps"></a>后续步骤
- [每个服务层级中有哪些可用资源](concepts-pricing-tiers.md)
- [支持的 MySQL 数据库版本](concepts-supported-versions.md)
