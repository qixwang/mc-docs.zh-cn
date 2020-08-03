---
title: 创建和更改外部 SQL 表 - Azure 数据资源管理器
description: 本文介绍如何创建和更改外部 SQL 表。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/24/2020
ms.date: 07/01/2020
ms.openlocfilehash: ccc01d2027b345999b56dbc39a420121d594d71e
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470383"
---
# <a name="create-and-alter-external-sql-tables"></a>创建和更改外部 SQL 表

在执行命令的数据库中创建或更改外部 SQL 表。

## <a name="syntax"></a>语法

(`.create` | `.alter`) `external` `table` _TableName_ ([columnName:columnType], ...)  
`kind` `=` `sql`  
`table` `=` _SqlTableName_  
`(`_SqlServerConnectionString_`)`  
[`with` `(`[`docstring` `=` _Documentation_][`,` `folder` `=` *foldername*], _property_name_ `=` _value_`,`...`)`]

## <a name="parameters"></a>parameters

- _TableName_ - 外部表名。 必须遵循[实体名称](../query/schema-entities/entity-names.md)的规则。 外部表不能与同一数据库中的常规表具有相同的名称。
- _SqlTableName_ - SQL 表的名称。
- _SqlServerConnectionString_ - SQL Server 的连接字符串。 可以是以下方法之一：
  - _AAD 集成身份验证_ (`Authentication="Active Directory Integrated"`)：用户或应用程序通过 AAD 向 Kusto 进行身份验证，然后使用同一令牌访问 SQL Server 网络终结点。
  - _用户名/密码身份验证_ (`User ID=...; Password=...;`)。 如果外部表用于[连续导出](data-export/continuous-data-export.md)，则必须使用此方法执行身份验证。

> [!WARNING]
> 应该对包含机密信息的连接字符串和查询进行模糊处理，使其在任何 Kusto 跟踪中都被忽略。 有关详细信息，请参阅[经过模糊处理的字符串文本](../query/scalar-data-types/string.md#obfuscated-string-literals)。

## <a name="optional-properties"></a>可选属性

| 属性            | 类型            | 说明                                                                                                                                                                                                                                                                                                                                 |
| ------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `folder`            | `string`        | 表的文件夹。                                                                                                                                                                                                                                                                                                                         |
| `docString`         | `string`        | 一个用来记录表的字符串。                                                                                                                                                                                                                                                                                                             |
| `firetriggers`      | `true`/`false`  | 如果为 `true`，则指示目标系统激发 SQL 表上定义的 INSERT 触发器。 默认为 `false`。 （有关详细信息，请参阅 [BULK INSERT](https://msdn.microsoft.com/library/ms188365.aspx) 和 [System.Data.SqlClient.SqlBulkCopy](<https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy(v=vs.110).aspx>)） |
| `createifnotexists` | `true`/ `false` | 如果为 `true`，则会在目标 SQL 表不存在的情况下创建该表；在这种情况下，必须提供 `primarykey` 属性来指示作为主键的结果列。 默认为 `false`。                                                                                                                         |
| `primarykey`        | `string`        | 如果 `createifnotexists` 为 `true`，则生成的列名称将用作 SQL 表的主键（如果是此命令创建的）。                                                                                                                                                                                                   |

> [!NOTE]
>
> - 如果该表存在，则 `.create` 命令会失败并显示错误。 使用 `.alter` 修改现有表。
> - 不支持更改外部 SQL 表的架构或格式。

需要[数据库用户权限](../management/access-control/role-based-authorization.md)（对于 `.create`）和[表管理员权限](../management/access-control/role-based-authorization.md)（对于 `.alter`）。

**示例**

```kusto
.create external table ExternalSql (x:long, s:string)
kind=sql
table=MySqlTable
(
   h@'Server=tcp:myserver.database.chinacloudapi.cn,1433;Authentication=Active Directory Integrated;Initial Catalog=mydatabase;'
)
with
(
   docstring = "Docs",
   folder = "ExternalTables",
   createifnotexists = true,
   primarykey = x,
   firetriggers=true
)
```

**输出**

| TableName   | TableType | 文件夹         | DocString | 属性                                                                                                                                                                                                                                                                                                                        |
| ----------- | --------- | -------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ExternalSql | Sql       | ExternalTables | Docs      | {<br> "TargetEntityKind": "sqltable`",<br> "TargetEntityName":"MySqlTable",<br> "TargetEntityConnectionString":"Server=tcp:myserver.database.chinacloudapi.cn,1433;Authentication=Active Directory Integrated;Initial Catalog=mydatabase;",<br> "FireTriggers": true,<br> "CreateIfNotExists": true,<br> "PrimaryKey": "x"<br>} |

## <a name="querying-an-external-table-of-type-sql"></a>查询 SQL 类型的外部表

支持查询外部 SQL 表。 请参阅[查询外部表](../../data-lake-query-data.md)。

> [!Note]
> SQL 外部表查询实现将从 SQL 表中执行完整的“SELECT \*”（或选择相关列）。 查询的其余部分将在 Kusto 端执行。

请考虑以下外部表查询：

```kusto
external_table('MySqlExternalTable') | count
```

Kusto 将对 SQL 数据库执行“SELECT \* from TABLE”查询，然后在 Kusto 端计数。
在这种情况下，如果直接用 T-SQL 来编写 ('SELECT COUNT(1) FROM TABLE') 并使用 [sql_request 插件](../query/sqlrequestplugin.md)来执行（而不是使用外部表函数），则性能会更好。
同样，筛选器不会推送到 SQL 查询。

如果查询需要读取整个表（或相关列）以便在 Kusto 端进一步执行，请使用外部表来查询 SQL 表。
如果 SQL 查询可在 T-SQL 中优化，请使用 [sql_request 插件](../query/sqlrequestplugin.md)。

## <a name="next-steps"></a>后续步骤

- [外部表常规控制命令](externaltables.md)
- [在 Azure 存储或 Azure Data Lake 中创建和更改外部表](external-tables-azurestorage-azuredatalake.md)
