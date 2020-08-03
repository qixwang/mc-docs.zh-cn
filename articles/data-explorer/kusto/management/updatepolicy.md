---
title: Kusto 对添加到源的数据的更新策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的更新策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 07/01/2020
ms.openlocfilehash: 62d94ad96ca8b050ab78046438c0dcfaab70b515
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470536"
---
# <a name="update-policy"></a>更新策略

“目标表”中设置了更新策略，用于指示 Kusto 在新数据插入“源表”中时自动向目标表追加数据 。 更新策略的查询针对插入源表中的数据运行。 这让你能够创建一个表作为另一个表的筛选视图，并且其中可能具有不同的架构、保留策略等。

默认情况下，无法运行更新策略不会影响将数据引入源表。 如果将更新策略定义为“事务性”，则无法运行更新策略会导致无法将数据引入源表。 （执行此操作时必须谨慎，因为某些用户错误，例如在更新策略中定义不正确的查询，可能会阻止将任何数据引入源表。）事务性更新策略的“边界”中引入的数据可用于单个事务中的查询。

更新策略的查询以特殊的模式运行，在该模式中，查询只能“看到”源表中新引入的数据。 无法在查询期间查询源表中已引入的数据。 这样做很快便会导致二次时引入。

由于更新策略是对目标表定义的，因此将数据引入一个源表可能会导致对该数据运行多个查询。 更新策略的执行顺序未明确定义。

例如，假设源表是高速率跟踪表，且其中值得关注的数据的格式为自由文本列。 另外，假定目标表（对其定义了更新策略）仅接受特定跟踪行，且具有结构良好的架构，该架构使用 Kusto 的 [parse 运算符](../query/parseoperator.md)转换初始自由文本数据。

更新策略的行为方式与常规引入类似，且遵循相同的限制和最佳做法。 例如，它可以随群集大小进行横向扩展，并且可以在大批量引入时更高效地工作。

## <a name="commands-that-trigger-the-update-policy"></a>触发更新策略的命令

使用以下任意命令将数据引入或移动到表中（创建盘区的位置）时，更新策略便会生效：

- [.ingest (pull)](../management/data-ingestion/ingest-from-storage.md)
- [.ingest (inline)](../management/data-ingestion/ingest-inline.md)
- [.set | .append | .set-or-append | .set-or-replace](../management/data-ingestion/ingest-from-query.md)
- [.move extents](../management/extents-commands.md#move-extents)
- [.replace extents](../management/extents-commands.md#replace-extents)

## <a name="the-update-policy-object"></a>更新策略对象

一个表可能具有零个、一个或多个与之相关联的更新策略对象。
每一个此类对象都表示为 JSON 属性包，并定义了以下属性：

| 属性                     | 类型     | 说明                                                                                                                                                                               |
| ---------------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IsEnabled                    | `bool`   | 说明是启用 (true) 还是禁用 (false) 更新策略                                                                                                                             |
| Source                       | `string` | 触发更新策略调用的表的名称                                                                                                                               |
| 查询                        | `string` | 用于生成更新数据的 Kusto CSL 查询                                                                                                                         |
| IsTransactional              | `bool`   | 说明更新策略是否是事务性的（默认为 false）。 无法运行事务性更新策略还会导致无法使用新数据对源表进行更新。 |
| PropagateIngestionProperties | `bool`   | 说明数据引入源表时指定的引入属性（盘区标签和创建时间）是否也应该应用于派生表中的属性。                |

## <a name="notes"></a>注释

- 查询将自动确定范围，仅涵盖新引入的记录。
- 查询可以调用存储的函数。
- 允许级联更新 (`TableA` → `TableB` → `TableC` → ...)
- 将更新策略作为 `.set-or-replace` 命令的一部分进行调用时，默认行为是派生表中的数据也会被替换，就像源表中的数据一样。

## <a name="limitations"></a>限制

- 源表和为其定义了更新策略的表必须位于同一数据库。
- 查询可能不包括跨数据库查询或跨群集查询。
- 如果以循环方式对多个表定义了更新策略，则会在运行时检测到这种情况，并切断更新链（即受影响的表链中的每个表只会引入数据一次）。
- 在策略的 `Query` 部分（或在后者引用的 Functions）中引用 `Source` 表时，请确保不要使用表的限定名称（即使用 `TableName`，而不是 `database("DatabaseName").TableName` 或 `cluster("ClusterName").database("DatabaseName").TableName`） 。
- 作为更新策略的一部分运行的查询无法读取启用了 [RestrictedViewAccess 策略](restrictedviewaccesspolicy.md)的表。
- 更新策略的查询无法引用启用了[行级别安全性策略](./rowlevelsecuritypolicy.md)的任何表。
- `PropagateIngestionProperties` 仅在引入操作时生效。 当更新策略作为 `.move extents` 或 `.replace extents` 命令的一部分触发时，此选项无效。

## <a name="retention-policy-on-the-source-table"></a>源表中的保留策略

为了不将原始数据保留在源表中，可以在源表的[保留策略](retentionpolicy.md)中将软删除周期设置为 0，同时将更新策略设置为事务性。

这将导致以下结果：

- 无法从源表查询源数据。
- 引入操作期间，源数据不会永久保存到持久性存储中。
- 操作性能得到改进。
- 由于在源表的[盘区](../management/extents-overview.md)中后台完成清理操作，引入后使用的资源将减少。

## <a name="failures"></a>失败数

在某些情况下，数据成功引入到源表中，但更新策略在数据引入到目标表时失败。

可以使用 [.show ingestion failures 命令](../management/ingestionfailures.md)检索更新策略时遇到的失败，如下所示：

```kusto
.show ingestion failures
| where FailedOn > ago(1hr) and OriginatesFromUpdatePolicy == true
```

失败的处理方式如下：

- **非事务性策略**：Kusto 将忽略此类失败。 任何重试均应由数据所有者负责。
- **事务性策略**：触发更新的初始引入操作也将失败。 源表和数据库不会被新数据修改。
  - 如果引入方法为 `pull`（引入过程涉及 Kusto 的数据管理服务），则根据以下逻辑，Kusto 的数据管理服务会自动重试整个引入操作：
    - 重试会一直进行，直到达到 `DataImporterMaximumRetryPeriod`（默认 = 2 天）或 `DataImporterMaximumRetryAttempts`（默认 = 10 次），以先达到的为准。
    - KustoOps 可以在数据管理服务的配置中更改以上两个设置。
    - 回退周期从 2 分钟开始，以指数形式增长（2 -> 4-> 8-> 16...分钟）
  - 在其他任何情况下，任何重试均应由数据所有者负责。

## <a name="control-commands"></a>控制命令

- 使用 [.show table TABLE policy update](../management/update-policy.md#show-update-policy) 显示表的当前更新策略。
- 使用 [.alter table TABLE policy update](../management/update-policy.md#alter-update-policy) 设置表的当前更新策略。
- 使用 [.alter-merge table TABLE policy update](../management/update-policy.md#alter-merge-table-table-policy-update) 追加到表的当前更新策略。
- 使用 [.delete table TABLE policy update](../management/update-policy.md#delete-table-table-policy-update) 追加到表的当前更新策略。

## <a name="testing-an-update-policys-performance-impact"></a>测试更新策略的性能影响

定义更新策略可能会影响 Kusto 群集的性能，因为它会影响对源表的任何数据引入。 强烈建议对更新策略的 `Query` 部分进行优化，使其更好地执行。
在创建或更改更新策略和/或它在 `Query` 部分中使用的函数之前，可以在特定的现有盘区中调用更新策略，以测试更新策略对引入操作的其他性能影响。

以下示例假设：

- 源表名称（更新策略的 `Source` 属性）为 `MySourceTable`。
- 更新策略的 `Query` 属性调用名为 `MyFunction()` 的函数。

可以使用 [.show queries](../management/queries.md) 评估以下查询的资源使用情况（CPU、内存等）和/或该查询的多次执行结果。

```kusto
.show table MySourceTable extents;
// The following line provides the extent ID for the not-yet-merged extent in the source table which has the most records
let extentId = $command_results | where MaxCreatedOn > ago(1hr) and MinCreatedOn == MaxCreatedOn | top 1 by RowCount desc | project ExtentId;
let MySourceTable = MySourceTable | where extent_id() == toscalar(extentId);
MyFunction()
```
