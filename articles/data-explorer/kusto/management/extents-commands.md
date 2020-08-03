---
title: 盘区（数据分片）管理 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的盘区（数据分片）管理。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 07/01/2020
ms.openlocfilehash: 5fbc6024c0587b6859e538f3dd6bc11412bf4aa6
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470389"
---
# <a name="extents-data-shards-management"></a>盘区（数据分片）管理

在 Kusto 中，数据分片被称为“盘区”，所有命令都将“extent”或“extents”作为同义词使用。

## <a name="show-extents"></a>.show 盘区

**群集级别**

`.show` `cluster` `extents` [`hot`]

显示有关群集中存在的盘区（数据分片）的信息。
如果指定了 `hot` - 仅显示应位于热缓存中的盘区。

**数据库级别**

`.show` `database` _DatabaseName_ `extents` [`(`_ExtentId1_`,`...`,`_ExtentIdN_`)`][`hot`] [`where` `tags` (`has`|`contains`|`!has`|`!contains`) _Tag1_ [`and` `tags` (`has`|`contains`|`!has`|`!contains`) *Tag2*...]]

显示有关指定数据库中存在的盘区（数据分片）的信息。
如果指定了 `hot` - 仅显示应位于热缓存中的盘区。

**表级别**

`.show` `table` _TableName_ `extents` [`(`_ExtentId1_`,`...`,`_ExtentIdN_`)`][`hot`] [`where` `tags` (`has`|`contains`|`!has`|`!contains`) _Tag1_ [`and` `tags` (`has`|`contains`|`!has`|`!contains`) *Tag2*...]]

`.show` `tables` `(`_TableName1_`,`...`,`_TableNameN_`)` `extents` [`(`_ExtentId1_`,`...`,`_ExtentIdN_`)`][`hot`] [`where` `tags` (`has`|`contains`|`!has`|`!contains`) _Tag1_ [`and` `tags` (`has`|`contains`|`!has`|`!contains`) *Tag2*...]]

显示有关指定表中存在的盘区（数据分片）的信息（数据库取自命令的上下文）。
如果指定了 `hot` - 仅显示应位于热缓存中的盘区。

**说明**

- 使用数据库和/或表级别命令比筛选（添加 `| where DatabaseName == '...' and TableName == '...'`）群集级别命令的结果要快得多。
- 如果提供了盘区 ID 的可选列表，则返回的数据集仅限于这些盘区。
  - 同样，这比筛选（将 `| where ExtentId in(...)` 添加到）“bare”命令的结果要快得多。
- 如果指定了 `tags` 筛选器：

  - 返回的列表仅限于标记集合遵循所有提供的标记筛选条件的盘区。
    - 同样，这比筛选（添加 `| where Tags has '...' and Tags contains '...'`）“bare”命令的结果要快得多。
  - `has` 筛选器是等值筛选器：未用任何指定标记进行标记的盘区将被筛选掉。
  - `!has` 筛选器是等值负筛选器：使用任一指定标记进行标记的盘区将被筛选掉。
  - `contains` 筛选器是不区分大小写的子字符串筛选器：所有标记的子字符串中都没有指定字符串的盘区将被筛选掉。
  - `!contains` 筛选器是不区分大小写的子字符串负筛选器：任一标记的子字符串中含有指定字符串的盘区将被筛选掉。

  - **示例**

    - 表 `T` 中的盘区 `E` 用标记 `aaa`、`BBB` 和 `ccc` 进行标记。
    - 此查询将返回 `E`：

    ```kusto
    .show table T extents where tags has 'aaa' and tags contains 'bb'
    ```

    - 此查询将不返回 `E`（因为它未使用等于 `aa` 的标记进行标记）：

    ```kusto
    .show table T extents where tags has 'aa' and tags contains 'bb'
    ```

    - 此查询将返回 `E`：

    ```kusto
    .show table T extents where tags contains 'aaa' and tags contains 'bb'
    ```

| 输出参数  | 类型     | 说明                                                                                                      |
| ----------------- | -------- | ---------------------------------------------------------------------------------------------------------------- |
| ExtentId          | Guid     | 盘区 ID                                                                                                 |
| DatabaseName      | String   | 盘区所属的数据库                                                                                  |
| TableName         | 字符串   | 盘区所属的表                                                                                     |
| MaxCreatedOn      | DateTime | 创建盘区时的日期时间（对于合并的盘区，则是源盘区中创建时间的最大值） |
| OriginalSize      | Double   | 盘区数据的原始大小（以字节为单位）                                                                        |
| ExtentSize        | Double   | 内存中的盘区大小（已压缩 + 索引）                                                                |
| CompressedSize    | Double   | 内存中盘区数据的压缩大小                                                                     |
| IndexSize         | Double   | 盘区数据的索引大小                                                                                    |
| 块            | Long     | 盘区中的数据块数量                                                                                  |
| 段数          | Long     | 盘区中的数据段数量                                                                                 |
| AssignedDataNodes | String   | 已弃用（空字符串）                                                                                     |
| LoadedDataNodes   | String   | 已弃用（空字符串）                                                                                     |
| ExtentContainerId | String   | 盘区所在的盘区容器的 ID                                                                      |
| RowCount          | Long     | 盘区中的行数                                                                                     |
| MinCreatedOn      | DateTime | 创建盘区时的日期时间（对于合并的盘区，则是源盘区中创建时间的最小值） |
| Tags              | String   | 为盘区定义的标记（如果有）                                                                             |

**示例**

```kusto
// Show volume of extents being created per hour in a specific database
.show database MyDatabase extents | summarize count(ExtentId) by MaxCreatedOn bin=time(1h) | render timechart

// Show volume of data arriving by table per hour
.show database MyDatabase extents
| summarize sum(OriginalSize) by TableName, MaxCreatedOn bin=time(1h)
| render timechart

// Show data size distribution by table
.show database MyDatabase extents | summarize sum(OriginalSize) by TableName

// Show all extents in the database named 'GamesDB'
.show database GamesDB extents

// Show all extents in the table named 'Games'
.show table Games extents

// Show all extents in the tables named 'TaggingGames1' and 'TaggingGames2', tagged with both 'tag1' and 'tag2'
.show tables (TaggingGames1,TaggingGames2) extents where tags has 'tag1' and tags has 'tag2'
```

## <a name="merge-extents"></a>.merge 盘区

**语法**

`.merge` `[async | dryrun]` _TableName_ `(` _GUID1_ [`,` *GUID2* ...] `)` `[with(rebuild=true)]`

此命令会合并盘区（请参阅：指定表中由其 ID 指示的[盘区（数据分片）概述](extents-overview.md)）。

合并操作有 2 种方式：合并（重新生成索引）和重新生成（完全重新引入数据） 。

- `async`：该操作将异步执行，结果会是一个操作 ID (GUID)，可以与 `.show operations <operation ID>` 一起运行，以查看命令的状态。
- `dryrun`：该操作将仅列出应合并的盘区，但实际上不会合并它们。
- `with(rebuild=true)`：将重新生成盘区（将重新引入数据）而不是合并（将重新生成索引）。

**返回输出**

| 输出参数 | 类型     | 说明                                                                                                                              |
| ---------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| OriginalExtentId | string   | 源表中原始盘区的唯一标识符 (GUID)，该标识符已合并到目标盘区。                    |
| ResultExtentId   | string   | 结果盘区的唯一标识符 (GUID)，该标识符从源盘区创建。 失败时 - “已失败”或“已弃用”。 |
| 持续时间         | timespan | 完成合并操作所需的时间段。                                                                                 |

**示例**

重新生成 `MyTable` 中的两个特定盘区，以异步方式执行

```kusto
.merge async MyTable (e133f050-a1e2-4dad-8552-1f5cf47cab69, 0d96ab2d-9dd2-4d2c-a45e-b24c65aa6687) with(rebuild=true)
```

合并 `MyTable` 中的两个特定盘区，以同步方式执行

```kusto
.merge MyTable (12345050-a1e2-4dad-8552-1f5cf47cab69, 98765b2d-9dd2-4d2c-a45e-b24c65aa6687)
```

**说明**

- 通常，`.merge` 命令应该很少手动运行，而应在 Kusto 群集的后台（根据为其中的表和数据库定义的[合并策略](mergepolicy.md)）持续自动执行。
  - [合并策略](mergepolicy.md)中记录了有关将多个盘区合并为一个盘区的标准的一般注意事项。
- `.merge` 操作的最终状态可能为 `Abandoned`（运行带有操作 ID 的 `.show operations` 时可以看到）- 此状态表明源盘区未合并到一起，因为源表中的某些源盘区已不再存在。
  - 在以下情况下可能会出现这种状态：
    - 源盘区已在表保留过程中被删除。
    - 源盘区已移到另一个表中。
    - 已完全删除或重命名源表。

## <a name="move-extents"></a>.move 盘区

**语法**

`.move` [`async`] `extents` `all` `from` `table` _SourceTableName_ `to` `table` _DestinationTableName_

`.move` [`async`] `extents` `(` _GUID1_ [`,` *GUID2* ...] `)` `from` `table` _SourceTableName_ `to` `table` _DestinationTableName_

`.move` [`async`] `extents` `to` `table` _DestinationTableName_ <| _query_

此命令在特定数据库的上下文中运行，并以事务方式将指定盘区从源表移动到目标表。
需要源表和目标表的[表管理员权限](../management/access-control/role-based-authorization.md)。

- `async`（可选）指定是否异步执行该命令（在这种情况下，将返回操作 ID (Guid)，并且可以使用 [.show operations](operations.md#show-operations) 命令来监视操作的状态）。
  - 如果使用此选项，则可以通过 [.show operation details](operations.md#show-operation-details) 命令检索成功执行的结果。

有三种方法可以指定要移动的盘区：

1. 特定表的所有盘区将被移动。
2. 在源表中明确指定盘区 ID。
3. 提供一个查询，该查询的结果指定源表中的盘区 ID。

**限制**

- 源表和目标表都必须位于上下文数据库中。
- 源表中的所有列均应以相同的名称和数据类型存在于目标表中。

**使用查询指定盘区**

```kusto
.move extents to table TableName <| ...query...
```

使用 Kusto 查询指定盘区，该查询将返回一个包含名为“ExtentId”的列的记录集。

**返回输出**（对于同步执行）

| 输出参数 | 类型   | 说明                                                                                                                                    |
| ---------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| OriginalExtentId | string | 源表中原始盘区的唯一标识符 (GUID)，该标识符已移至目标表。                         |
| ResultExtentId   | string | 结果盘区的唯一标识符 (GUID)，该标识符已从源表移至目标表。 失败时 -“已失败”。 |
| 详细信息          | string | 包括失败详细信息（如果操作失败）。                                                                                     |

**示例**

将表 `MyTable` 中的所有盘区都移至表 `MyOtherTable`。

```kusto
.move extents all from table MyTable to table MyOtherTable
```

将 2 个特定盘区（通过其盘区 ID）从表 `MyTable` 移至表 `MyOtherTable`。

```kusto
.move extents (AE6CD250-BE62-4978-90F2-5CB7A10D16D7,399F9254-4751-49E3-8192-C1CA78020706) from table MyTable to table MyOtherTable
```

将所有盘区从 2 个特定表（`MyTable1`、`MyTable2`）移至表 `MyOtherTable`。

```kusto
.move extents to table MyOtherTable <| .show tables (MyTable1,MyTable2) extents
```

**示例输出**

| OriginalExtentId                     | ResultExtentId                       | 详细信息 |
| ------------------------------------ | ------------------------------------ | ------- |
| e133f050-a1e2-4dad-8552-1f5cf47cab69 | 0d96ab2d-9dd2-4d2c-a45e-b24c65aa6687 |
| cdbeb35b-87ea-499f-b545-defbae091b57 | a90a303c-8a14-4207-8f35-d8ea94ca45be |
| 4fcb4598-9a31-4614-903c-0c67c286da8c | 97aafea1-59ff-4312-b06b-08f42187872f |
| 2dfdef64-62a3-4950-a130-96b5b1083b5a | 0fb7f3da-5e28-4f09-a000-e62eb41592df |

## <a name="drop-extents"></a>.drop 盘区

从指定的数据库/表中删除盘区。
该命令有多个变体：在一个变体中，要删除的盘区是由 Kusto 查询指定的，而在其他变体中，盘区则是使用下面所述的迷你语言指定的。

### <a name="specifying-extents-with-a-query"></a>使用查询指定盘区

对于每个具有由提供的查询所返回的盘区的表，需要[表管理员权限](../management/access-control/role-based-authorization.md)。

删除盘区（或者当使用 `whatif` 时，仅报告它们，而不删除它们）：

**语法**

`.drop` `extents` [`whatif`] <| _query_

使用 Kusto 查询指定盘区，该查询将返回一个包含名为“ExtentId”的列的记录集。

### <a name="dropping-a-specific-extent"></a>删除特定盘区

如果已指定表名，则需要[表管理员权限](../management/access-control/role-based-authorization.md)。

如果未指定表名，则需要[数据库管理员权限](../management/access-control/role-based-authorization.md)。

**语法**

`.drop` `extent` _ExtentId_ [`from` *TableName*]

### <a name="dropping-specific-multiple-extents"></a>删除特定的多个盘区

如果已指定表名，则需要[表管理员权限](../management/access-control/role-based-authorization.md)。

如果未指定表名，则需要[数据库管理员权限](../management/access-control/role-based-authorization.md)。

**语法**

`.drop` `extents` `(`_ExtentId1_`,`..._ExtentIdN_`)` [`from` *TableName*]

### <a name="specifying-extents-by-properties"></a>按属性指定盘区

如果已指定表名，则需要[表管理员权限](../management/access-control/role-based-authorization.md)。

如果未指定表名，则需要[数据库管理员权限](../management/access-control/role-based-authorization.md)。

`.drop` `extents` [`older` *N* (`days` | `hours`)] `from` (_TableName_ | `all` `tables`) [`trim` `by` (`extentsize` | `datasize`) _N_ (`MB` | `GB` | `bytes`)][`limit` *limitcount*]

- `older`：仅会删除早于 N 天/小时的盘区。
- `trim`：该操作将修整数据库中的数据，直到盘区总数与所需大小 (MaxSize) 相匹配
- `limit`：该操作将应用于第一个 _LimitCount_ 盘区

此命令支持仿真（`.drop-pretend` 而不是 `.drop`）模式，该模式生成输出，就好像该命令已运行，但实际上并没有执行。

**示例**

从数据库 `MyDatabase` 的所有表中删除创建了 10 天以上的所有盘区

```kusto
.drop extents <| .show database MyDatabase extents | where CreatedOn < now() - time(10d)
```

删除表 `Table1` 和 `Table2` 中创建时间超过 10 天的所有盘区：

```kusto
.drop extents older 10 days from tables (Table1, Table2)
```

仿真模式：显示该命令将删除哪些盘区（盘区 ID 参数不适用于此命令）：

```kusto
.drop-pretend extents older 10 days from all tables
```

从“TestTable”中删除所有盘区：

```kusto
.drop extents from TestTable
```

**返回输出**

| 输出参数 | 类型     | 说明                                                              |
| ---------------- | -------- | ------------------------------------------------------------------------ |
| ExtentId         | String   | 由该命令删除的 ExtentId                     |
| TableName        | 字符串   | 盘区所属的表名                                        |
| CreatedOn        | DateTime | 保存有关最初创建盘区的时间信息的时间戳 |

**示例输出**

| 盘区 ID                            | 表名称 | 创建时间                  |
| ------------------------------------ | ---------- | --------------------------- |
| 43c6e03f-1713-4ca7-a52a-5db8a4e8b87d | TestTable  | 2015-01-12 12:48:49.4298178 |

## <a name="replace-extents"></a>.replace 盘区

**语法**

`.replace` [`async`] `extents` `in` `table` _DestinationTableName_ `<| {`_查询要从表中删除的盘区_`},{`_查询要移动到表中的盘区_`}`

此命令在特定数据库的上下文中运行，将指定盘区从其源表移至目标表，并从目标表中删除指定的盘区。
所有删除和移动操作都在单个事务中完成。

需要源表和目标表的[表管理员权限](../management/access-control/role-based-authorization.md)。

- `async`（可选）指定是否异步执行该命令（在这种情况下，将返回操作 ID (Guid)，并且可以使用 [.show operations](operations.md#show-operations) 命令来监视操作的状态）。
  - 如果使用此选项，则可以通过 [.show operation details](operations.md#show-operation-details) 命令检索成功执行的结果。

通过提供 2 个查询来指定应删除或移动的盘区

- 查询要从表中删除的盘区 - 此查询的结果指定应从目标表中删除  
  的盘区 ID。
- 查询要移至表的盘区 - 此查询的结果指定源表中应移至目标表的盘区 ID。

这两个查询都应返回一个包含名为“ExtentId”的列的记录集。

**限制**

- 源表和目标表都必须位于上下文数据库中。
- 对要从表中删除的盘区的查询所指定的所有盘区应属于目标表。
- 源表中的所有列均应以相同的名称和数据类型存在于目标表中。

**返回输出**（对于同步执行）

| 输出参数 | 类型   | 说明                                                                                                                                                                                                           |
| ---------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OriginalExtentId | string | 源表中原始盘区的唯一标识符 (GUID)（已移至目标表），或者目标表中盘区的唯一标识符（已删除）。                              |
| ResultExtentId   | string | 已从源表移至目标表的结果盘区的唯一标识符 (GUID)，或为空（如果已从目标表中删除了该盘区）。 失败时 -“已失败”。 |
| 详细信息          | string | 包括失败详细信息（如果操作失败）。                                                                                                                                                            |

> [!NOTE]
> 如果目标表中不存在要从表中删除的盘区的查询所返回的盘区，则该命令将失败。 如果在执行 replace 命令之前合并了盘区，则可能会发生这种情况。
> 为确保命令在缺失盘区时失败，请检查查询是否返回所需的盘区 ID。 如果表 MyOtherTable 中不存在要删除的盘区，则下面的示例 #1 将会失败。 但是，即使要删除的盘区不存在，示例 #2 也会成功，因为删除查询未返回任何盘区 ID。

**示例**

以下命令将所有盘区从 2 个特定表（`MyTable1`、`MyTable2`）移至表 `MyOtherTable`，并删除 `MyOtherTable` 中用 `drop-by:MyTag` 标记的所有盘区：

```kusto
.replace extents in table MyOtherTable <|
    {
        .show table MyOtherTable extents where tags has 'drop-by:MyTag'
    },
    {
        .show tables (MyTable1,MyTable2) extents
    }
```

**示例输出**

| OriginalExtentId                     | ResultExtentId                       | 详细信息 |
| ------------------------------------ | ------------------------------------ | ------- |
| e133f050-a1e2-4dad-8552-1f5cf47cab69 | 0d96ab2d-9dd2-4d2c-a45e-b24c65aa6687 |
| cdbeb35b-87ea-499f-b545-defbae091b57 | a90a303c-8a14-4207-8f35-d8ea94ca45be |
| 4fcb4598-9a31-4614-903c-0c67c286da8c | 97aafea1-59ff-4312-b06b-08f42187872f |
| 2dfdef64-62a3-4950-a130-96b5b1083b5a | 0fb7f3da-5e28-4f09-a000-e62eb41592df |

以下命令将所有盘区从 1 个特定表 (`MyTable1`) 移至表 `MyOtherTable`，并按其 ID 在 `MyOtherTable` 中删除特定盘区：

```kusto
.replace extents in table MyOtherTable <|
    {
        print ExtentId = "2cca5844-8f0d-454e-bdad-299e978be5df"
    },
    {
        .show table MyTable1 extents
    }
```

```kusto
.replace extents in table MyOtherTable  <|
    {
        .show table MyOtherTable extents
        | where ExtentId == guid(2cca5844-8f0d-454e-bdad-299e978be5df)
    },
    {
        .show table MyTable1 extents
    }
```

下面的命令实现幂等逻辑，因此仅在有盘区从表 `t_source` 移至表 `t_dest` 时，才从表 `t_dest` 中删除盘区：

```kusto
.replace async extents in table t_dest <|
{
    let any_extents_to_move = toscalar(
        t_source
        | where extent_tags() has 'drop-by:blue'
        | summarize count() > 0
    );
    let extents_to_drop =
        t_dest
        | where any_extents_to_move and extent_tags() has 'drop-by:blue'
        | summarize by ExtentId = extent_id()
    ;
    extents_to_drop
},
{
    let extents_to_move =
        t_source
        | where extent_tags() has 'drop-by:blue'
        | summarize by ExtentId = extent_id()
    ;
    extents_to_move
}
```

## <a name="drop-extent-tags"></a>.drop 盘区标记

**语法**

`.drop` [`async`] `extent` `tags` `from` `table` _TableName_ `(`'_Tag1_'[`,`'*Tag2*'`,`...`,`'*TagN*']`)`

`.drop` [`async`] `extent` `tags` <| _query_

- `async`（可选）指定是否异步执行该命令（在这种情况下，将返回操作 ID (Guid)，并且可以使用 [.show operations](operations.md#show-operations) 命令来监视操作的状态）。
  - 如果使用此选项，则可以通过 [.show operation details](operations.md#show-operation-details) 命令检索成功执行的结果。

此命令在特定数据库的上下文中运行，并从所提供的（包含任意标记的）数据库和表中的任何盘区中删除提供的[盘区标记](extents-overview.md#extent-tagging)。

可以通过两种方式来指定应从哪些盘区中删除哪些标记：

1. 在指定表中明确指定应从所有盘区中删除的标记。
2. 提供一个查询，该查询的结果指定表中的盘区 ID，并为每个盘区指定应删除的标记。

**限制**

- 所有盘区都必须在上下文数据库中，并且必须属于同一个表。

**使用查询指定盘区**

需要所有涉及的源表和目标表的[表管理员权限](../management/access-control/role-based-authorization.md)。

```kusto
.drop extent tags <| ...query...
```

使用 Kusto 查询指定要删除的盘区和标记，该查询将返回一个记录集，其中包含名为“ExtentId”的列和名为“Tags”的列。

_注意_：使用 [Kusto .NET 客户端库](../api/netfx/about-kusto-data.md)时，可以使用以下方法来生成所需的命令：

- `CslCommandGenerator.GenerateExtentTagsDropByRegexCommand(string tableName, string regex)`
- `CslCommandGenerator.GenerateExtentTagsDropBySubstringCommand(string tableName, string substring)`

**返回输出**

| 输出参数 | 类型   | 说明                                                                                                                                            |
| ---------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| OriginalExtentId | string | 原始盘区的唯一标识符 (GUID)，其标记已修改（并在操作过程中删除）                             |
| ResultExtentId   | string | 结果盘区的唯一标识符 (GUID)，其标记已修改（并在操作过程中创建和添加）。 失败时 -“已失败”。 |
| ResultExtentTags | string | 结果盘区所标记的标记的集合（如果还有），如果操作失败，则为“null”。                                  |
| 详细信息          | string | 包括失败详细信息（如果操作失败）。                                                                                             |

**示例**

在表 `MyOtherTable` 中，从标记了 `drop-by:Partition000` 标记的任何盘区中删除该标记。

```kusto
.drop extent tags from table MyOtherTable ('drop-by:Partition000')
```

在表 `My Table` 中，从标记了任一 `drop-by:20160810104500`、`a random tag` 和/或 `drop-by:20160810` 标记的任何盘区中删除该标记。

```kusto
.drop extent tags from table [My Table] ('drop-by:20160810104500','a random tag','drop-by:20160810')
```

从表 `MyTable` 的盘区中删除所有 `drop-by` 标记。

```kusto
.drop extent tags <|
  .show table MyTable extents
  | where isnotempty(Tags)
  | extend Tags = split(Tags, '\r\n')
  | mv-expand Tags to typeof(string)
  | where Tags startswith 'drop-by'
```

从表 `MyTable` 的盘区中删除与正则表达式 `drop-by:StreamCreationTime_20160915(\d{6})` 相匹配的所有标记。

```kusto
.drop extent tags <|
  .show table MyTable extents
  | where isnotempty(Tags)
  | extend Tags = split(Tags, '\r\n')
  | mv-expand Tags to typeof(string)
  | where Tags matches regex @"drop-by:StreamCreationTime_20160915(\d{6})"
```

**示例输出**

| OriginalExtentId                     | ResultExtentId                       | ResultExtentTags          | 详细信息 |
| ------------------------------------ | ------------------------------------ | ------------------------- | ------- |
| e133f050-a1e2-4dad-8552-1f5cf47cab69 | 0d96ab2d-9dd2-4d2c-a45e-b24c65aa6687 | Partition001              |
| cdbeb35b-87ea-499f-b545-defbae091b57 | a90a303c-8a14-4207-8f35-d8ea94ca45be |                           |
| 4fcb4598-9a31-4614-903c-0c67c286da8c | 97aafea1-59ff-4312-b06b-08f42187872f | Partition001 Partition002 |
| 2dfdef64-62a3-4950-a130-96b5b1083b5a | 0fb7f3da-5e28-4f09-a000-e62eb41592df |                           |

## <a name="alter-extent-tags"></a>.alter 盘区标记

**语法**

`.alter` [`async`] `extent` `tags` `(`'_Tag1_'[`,`'*Tag2*'`,`...`,`'*TagN*']`)` <| _query_

此命令在特定数据库的上下文中运行，将指定查询返回的所有盘区的[盘区标记](extents-overview.md#extent-tagging)更改为所提供的一组标记。

使用 Kusto 查询指定要更改的盘区和标记，该查询将返回一个包含名为“ExtentId”的列的记录集。

- `async`（可选）指定是否异步执行该命令（在这种情况下，将返回操作 ID (Guid)，并且可以使用 [.show operations](operations.md#show-operations) 命令来监视操作的状态）。
  - 如果使用此选项，则可以通过 [.show operation details](operations.md#show-operation-details) 命令检索成功执行的结果。

需要所有涉及的表的[表管理员权限](../management/access-control/role-based-authorization.md)。

**限制**

- 所有盘区都必须在上下文数据库中，并且必须属于同一个表。

**返回输出**

| 输出参数 | 类型   | 说明                                                                                                                                            |
| ---------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| OriginalExtentId | string | 原始盘区的唯一标识符 (GUID)，其标记已修改（并在操作过程中删除）                             |
| ResultExtentId   | string | 结果盘区的唯一标识符 (GUID)，其标记已修改（并在操作过程中创建和添加）。 失败时 -“已失败”。 |
| ResultExtentTags | string | 结果盘区所标记的标记的集合，如果操作失败，则为“null”。                                                  |
| 详细信息          | string | 包括失败详细信息（如果操作失败）。                                                                                             |

**示例**

将表 `MyTable` 中所有盘区的标记更改为 `MyTag`。

```kusto
.alter extent tags ('MyTag') <| .show table MyTable extents
```

将表 `MyTable` 中所有标记为 `drop-by:MyTag` 的盘区的标记更改为 `drop-by:MyNewTag` 和 `MyOtherNewTag`。

```kusto
.alter extent tags ('drop-by:MyNewTag','MyOtherNewTag') <| .show table MyTable extents where tags has 'drop-by:MyTag'
```

**示例输出**

| OriginalExtentId                     | ResultExtentId                       | ResultExtentTags               | 详细信息 |
| ------------------------------------ | ------------------------------------ | ------------------------------ | ------- |
| e133f050-a1e2-4dad-8552-1f5cf47cab69 | 0d96ab2d-9dd2-4d2c-a45e-b24c65aa6687 | drop-by:MyNewTag MyOtherNewTag |
| cdbeb35b-87ea-499f-b545-defbae091b57 | a90a303c-8a14-4207-8f35-d8ea94ca45be | drop-by:MyNewTag MyOtherNewTag |
| 4fcb4598-9a31-4614-903c-0c67c286da8c | 97aafea1-59ff-4312-b06b-08f42187872f | drop-by:MyNewTag MyOtherNewTag |
| 2dfdef64-62a3-4950-a130-96b5b1083b5a | 0fb7f3da-5e28-4f09-a000-e62eb41592df | drop-by:MyNewTag MyOtherNewTag |
