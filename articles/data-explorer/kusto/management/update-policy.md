---
title: Kusto 更新策略管理 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的更新策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 07/01/2020
ms.openlocfilehash: 3f460fd2cf0db39c813c3b74aaea81902a1a73fc
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470438"
---
# <a name="update-policy"></a>更新策略

[更新策略](updatepolicy.md)是一个表级别策略对象，它自动运行查询，然后在将数据引入到另一个表时引入结果。

## <a name="show-update-policy"></a>显示更新策略

如果将 `*` 用作表名称，此命令将返回指定表或默认数据库中所有表的更新策略。

**语法**

- `.show` `table` TableName `policy` `update`
- `.show` `table` DatabaseName`.`TableName `policy` `update` 
- `.show` `table` `*` `policy` `update`

**返回**

此命令返回每个表都有一条记录的表。

| 列     | 类型     | 说明                                                                                                                                       |
| ---------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| EntityName | `string` | 对其定义更新策略的实体的名称                                                                                            |
| 策略   | `string` | 一个 JSON 数组，指示为该实体定义的所有更新策略，格式设置为[更新策略对象](updatepolicy.md#the-update-policy-object) |

**示例**

```kusto
.show table DerivedTableX policy update
```

| EntityName               | 策略                                                                                                                                 |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| [TestDB].[DerivedTableX] | [{"IsEnabled": true, "Source":"MyTableX","Query":"MyUpdateFunction()","IsTransactional": false,"PropagateIngestionProperties": false}] |

## <a name="alter-update-policy"></a>更改更新策略

此命令设置指定表的更新策略。

**语法**

- `.alter` `table` TableName `policy` `update` ArrayOfUpdatePolicyObjects 
- `.alter` `table` DatabaseName`.`TableName `policy` `update` ArrayOfUpdatePolicyObjects  

ArrayOfUpdatePolicyObjects 是一个定义了零个或多个更新策略对象的 JSON 数组。

> [!NOTE]
>
> - 对更新策略对象的 `Query` 属性使用存储的函数。
>   只需修改函数定义，而不是整个策略对象。
> - 如果 `IsEnabled` 设置为 `true`，则在设置更新策略时对其执行以下验证：
>   - `Source` - 检查目标数据库中是否存在该表。
>   - `Query`
>     - 检查架构定义的架构是否与目标表中的架构匹配
>     - 检查查询是否引用更新策略的 `source` 表。

        Defining an update policy query which does *not* reference the source is possible by setting
        `AllowUnreferencedSourceTable=true` in the *with* properties (see example below),
        but isn't recommended due to performance issues. For every ingestion to the source table,
        *all* records in a different table are considered for the update policy execution.

>       * Checks that the policy doesn't result in a cycle being created in the chain of update policies in the target database.
>
> - 如果 `IsTransactional` 设置为 `true`，则检查 `TableAdmin` 权限是否也针对 `Source`（源表）进行了验证。
> - 测试更新策略或函数的性能，然后应用它以在源表每次引入时运行。 有关详细信息，请参阅[测试更新策略的性能影响](updatepolicy.md#testing-an-update-policys-performance-impact)。

**返回**

此命令设置表的更新策略对象，替代所有当前策略，然后返回相应 [.show table TABLE update policy](#show-update-policy) 命令的输出。

**示例**

```kusto
// Create a function that will be used for update
.create function
MyUpdateFunction()
{
    MyTableX
    | where ColumnA == 'some-string'
    | summarize MyCount=count() by ColumnB, Key=ColumnC
    | join (OtherTable | project OtherColumnZ, Key=OtherColumnC) on Key
    | project ColumnB, ColumnZ=OtherColumnZ, Key, MyCount
}

// Create the target table (if it doesn't already exist)
.set-or-append DerivedTableX <| MyUpdateFunction() | limit 0

// Use update policy on table DerivedTableX
.alter table DerivedTableX policy update
@'[{"IsEnabled": true, "Source": "MyTableX", "Query": "MyUpdateFunction()", "IsTransactional": false, "PropagateIngestionProperties": false}]'
```

- 当发生对源表（在本例中为 `MyTableX`）的引入时，在该表中创建一个或多个盘区（数据分片）
- 在更新策略对象（在本例中为 `MyUpdateFunction()）中定义的 `Query` 将仅在这些盘区上运行，而不会在整个表上运行。
  - “范围界定”是在内部自动完成的，在定义 `Query` 时不应进行处理。
  - 当引入到 `DerivedTableX` 派生表时，仅考虑每个引入操作中不同的新引入记录。

```kusto
// The following example will throw an error for not referencing the source table in the update policy query
// The policy's source table is MyTableX, whereas the query only references MyOtherTable.
.alter table DerivedTableX policy update
@'[{"IsEnabled": true, "Source": "MyTableX", "Query": "MyOtherTable", "IsTransactional": false, "PropagateIngestionProperties": false}]'

// Adding the following properties will suppress the error (but is not recommended)
.alter table DerivedTableX policy update with (AllowUnreferencedSourceTable=true)
@'[{"IsEnabled": true, "Source": "MyTableX", "Query": "MyOtherTable", "IsTransactional": false, "PropagateIngestionProperties": false}]'

```

## <a name="alter-merge-table-table-policy-update"></a>.alter-merge table TABLE policy update

此命令修改指定表的更新策略。

**语法**

- `.alter-merge` `table` TableName `policy` `update` ArrayOfUpdatePolicyObjects 
- `.alter-merge` `table` DatabaseName`.`TableName `policy` `update` ArrayOfUpdatePolicyObjects  

ArrayOfUpdatePolicyObjects 是一个定义了零个或多个更新策略对象的 JSON 数组。

> [!NOTE]
>
> - 对批量实现更新策略对象的查询属性使用存储的函数。

     You'll only need to modify the function definition instead of the entire policy object.

> - 该验证与对 `alter` 命令执行的验证相同。

**返回**

此命令追加到表的更新策略对象，替代所有当前策略，然后返回相应 [.show table TABLE update policy](#show-update-policy) 命令的输出。

**示例**

```kusto
.alter-merge table DerivedTableX policy update
@'[{"IsEnabled": true, "Source": "MyTableY", "Query": "MyUpdateFunction()", "IsTransactional": false}]'
```

## <a name="delete-table-table-policy-update"></a>.delete table TABLE policy update

删除指定表的更新策略。

**语法**

- `.delete` `table` TableName `policy` `update`
- `.delete` `table` DatabaseName`.`TableName `policy` `update` 

**返回**

此命令删除表的更新策略对象，然后返回相应 [.show table TABLE update policy](#show-update-policy) 命令的输出。

**示例**

```kusto
.delete table DerivedTableX policy update
```
