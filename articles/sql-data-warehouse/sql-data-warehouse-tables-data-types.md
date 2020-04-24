---
title: 定义数据类型 - Azure SQL 数据仓库 | Microsoft Docs
description: 有关在 Azure SQL 数据仓库中定义表数据类型的建议。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
origin.date: 04/17/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 65e055c60e823ad4e6f56fe7a59fe08bb608cf9f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63860518"
---
# <a name="table-data-types-in-azure-sql-data-warehouse"></a>Azure SQL 数据仓库中的表数据类型
有关在 Azure SQL 数据仓库中定义表数据类型的建议。 

## <a name="what-are-the-data-types"></a>数据类型有哪些？

SQL 数据仓库支持最常用的数据类型。 有关受支持数据类型的列表，请参阅 CREATE TABLE 语句中的[数据类型](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes)。 

## <a name="minimize-row-length"></a>最大限度缩短行长度
最大限度减小数据类型大小可缩短行长度，获得更好的查询性能。 使用适用于数据的最小数据类型。 

- 避免使用较大默认长度定义字符列。 例如，如果最长的值是 25 个字符，则将列定义为 VARCHAR(25)。 
- 当仅需要 VARCHAR 时请避免使用 [NVARCHAR][NVARCHAR]
- 尽可能使用 NVARCHAR(4000) 或 VARCHAR(8000)，而非 NVARCHAR(MAX) 或 VARCHAR(MAX)。

如果使用 PolyBase 外部表来加载表，则定义的表行长度不能超过 1 MB。 当数据长度可变的行超过 1 MB 时，可使用 BCP 而不是 PolyBase 加载行。

## <a name="identify-unsupported-data-types"></a>识别不支持的数据类型
如果从另一个 SQL 数据库迁移数据库，可能会遇到 SQL 数据仓库不支持的数据类型。 使用此查询发现现有 SQL 架构中不支持的数据类型。

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>对不受支持的数据类型的解决方法

以下列表显示 SQL 数据仓库不支持的数据类型，同时提供可替代不支持的数据类型的可用数据类型。

| 不支持的数据类型 | 解决方法 |
| --- | --- |
| [geometry](https://docs.microsoft.com/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql) |[varbinary](https://docs.microsoft.com/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [地理](https://docs.microsoft.com/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](https://docs.microsoft.com/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [hierarchyid](https://docs.microsoft.com/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)(4000) |
| [图像](https://docs.microsoft.com/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varbinary](https://docs.microsoft.com/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [text](https://docs.microsoft.com/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varchar](https://docs.microsoft.com/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [ntext](https://docs.microsoft.com/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[nvarchar](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql) |
| [sql_variant](https://docs.microsoft.com/sql/t-sql/data-types/sql-variant-transact-sql) |将列拆分成多个强类型化列。 |
| [table](https://docs.microsoft.com/sql/t-sql/data-types/table-transact-sql) |转换成暂时表。 |
| [timestamp](https://docs.microsoft.com/sql/t-sql/data-types/date-and-time-types) |重写代码来使用 [datetime2](https://docs.microsoft.com/sql/t-sql/data-types/datetime2-transact-sql) 和 [CURRENT_TIMESTAMP](https://docs.microsoft.com/sql/t-sql/functions/current-timestamp-transact-sql) 函数。 仅支持常量作为默认值，因此，不能将 current_timestamp 定义为默认约束。 如果需要从 timestamp 类型化列迁移行版本值，请为 NOT NULL 或 NULL 行版本值使用 [BINARY](https://docs.microsoft.com/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) 或 [VARBINARY](https://docs.microsoft.com/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8)。 |
| [xml](https://docs.microsoft.com/sql/t-sql/xml/xml-transact-sql) |[varchar](https://docs.microsoft.com/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [用户定义的类型](https://docs.microsoft.com/sql/relational-databases/native-client/features/using-user-defined-types) |尽可能转换回本机数据类型。 |
| 默认值 | 默认值仅支持文本和常量。 |


## <a name="next-steps"></a>后续步骤
有关开发表的详细信息，请参阅[表概述](sql-data-warehouse-tables-overview.md)。
