---
title: 连续数据导出 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的连续数据导出。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: yifats
ms.service: data-explorer
ms.topic: reference
origin.date: 03/27/2020
ms.date: 07/01/2020
ms.openlocfilehash: deb870c07cfcb1af044ec7c27a92f1bb490a5cb2
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226274"
---
# <a name="continuous-data-export"></a>连续数据导出

连续将数据从 Kusto 导出到[外部表](../externaltables.md)。 外部表定义了导出的数据的目标（例如 Azure Blob 存储）和架构。 导出的数据是由定期运行的查询定义的。 结果将存储在外部表中。 此过程可确保所有记录都导出“恰好一次”（不包括维度表，在所有执行中都会对维度表中的所有记录进行评估）。 

连续数据导出要求[创建外部表](../external-tables-azurestorage-azuredatalake.md#create-or-alter-external-table)，然后[创建连续导出定义](#create-or-alter-continuous-export)并将其指向外部表。 

> [!NOTE] 
> * Kusto 不支持导出在创建连续导出（此创建操作是连续导出过程的一部分）前引入的历史记录。 可以使用（非连续）[导出命令](export-data-to-an-external-table.md)单独导出历史记录。 有关详细信息，请参阅[导出历史数据](#exporting-historical-data)。
> * 连续导出对于通过流式引入引入的数据不起作用。 
> * 目前，无法在启用了[行级别安全性策略](../../management/rowlevelsecuritypolicy.md)的表上配置连续导出。
> * 其[连接字符串](../../api/connection-strings/storage.md)中包含 `impersonate` 的外部表不支持连续导出。
> * 如果连续导出所用的项目用于触发事件网格通知，请参阅[事件网格文档中的“已知问题”部分](../data-ingestion/eventgrid.md#known-issues)。

## <a name="notes"></a>注释

* 导出“恰好一次”的保证仅适用于[显示导出的项目命令](#show-continuous-export-artifacts)中报告的文件。 
连续导出并不保证仅将每个记录向外部表中写入一次。 如果在开始导出后发生故障，并且某些项目已被写入到外部表，则外部表可能包含重复项（如果写入操作在完成前被中止，甚至会包含损坏的文件）。 在这种情况下，不会从外部表中删除这些项目，但也不会在[显示导出的项目命令](#show-continuous-export-artifacts)中报告这些项目。 通过 `show exported artifacts command` 使用导出的文件。 
 保证没有重复项（不是损坏项）。
* 为了保证导出“恰好一次”，连续导出使用[数据库游标](../databasecursor.md)。 
必须在所有已在查询中引用且应当在导出中处理“恰好一次”的表上启用 [IngestionTime 策略](../ingestiontime-policy.md)。 此策略默认在所有新创建的表上启用。
* 导出查询的输出架构必须与要导出到的外部表的架构匹配。 
* 连续导出不支持跨数据库/群集调用。
* 连续导出将根据为其配置的时间段运行。 此间隔的建议值至少为几分钟，具体取决于你愿意接受的延迟时间。 连续导出不是设计用于将数据从 Kusto 连续流式传输出去。 它以分布式模式运行，所有节点并发导出。 如果每个运行所查询的数据范围很小，则连续导出的输出将是很多小项目（数量取决于群集中的节点数）。 
* 可以并发运行的导出操作的数量受限于群集的数据导出容量（请参阅[限制](../../management/capacitypolicy.md#throttling)）。 如果群集没有足够的容量来处理所有连续导出，某些导出将会滞后启动。 
 
* 默认情况下，会假定导出查询中引用的所有表都是[事实数据表](../../concepts/fact-and-dimension-tables.md)。 
因此，它们的作用域限定于数据库游标。 导出查询仅包含自上一次执行导出后联接的记录。 
导出查询可能包含[维度表](../../concepts/fact-and-dimension-tables.md)，维度表的所有记录都包括在所有导出查询中。  
    * 在连续导出中的事实数据表与维度表之间使用联接时，必须注意，事实数据表中的记录仅处理一次 - 如果在运行导出时维度表中缺少某些键的记录，则导出的文件会缺少相应键的记录或者会在维度列中包含 null 值（具体取决于查询使用内部联接还是外部联接）。 对于此类情况，连续导出定义中的 forcedLatency 属性可能非常有用，因为对于匹配的记录，事实数据表和维度表是在同一时间内引入的。
    * 不支持纯维度表的连续导出。 导出查询必须包含至少一个事实数据表。
    * 此语法显式声明哪些表具有作用域（事实数据表）以及哪些表没有作用域（维度表）。 有关详细信息，请参阅[创建命令](#create-or-alter-continuous-export)中的 `over` 参数。

* 每个连续导出迭代中导出的文件数取决于外部表的分区情况。 有关详细信息，请参阅[导出到外部表命令](export-data-to-an-external-table.md)中的“说明”部分。 每个连续导出迭代始终会写入到新文件中，永远不会附加到现有文件中。 因此，导出的文件的数目也取决于连续导出的运行频率（`intervalBetweenRuns` 参数）。

所有连续导出命令都需要[数据库管理员权限](../access-control/role-based-authorization.md)。

## <a name="create-or-alter-continuous-export"></a>创建或更改连续导出

**语法：**

`.create-or-alter` `continuous-export` *ContinuousExportName* <br>
[ `over` `(`*T1*, *T2* `)`] <br>
`to` `table` *ExternalTableName* <br> [ `with` `(`*PropertyName* `=` *PropertyValue*`,`...`)`]<br>
\<| *Query*

**属性**：

| 属性             | 类型     | 说明   |
|----------------------|----------|---------------------------------------|
| ContinuousExportName | String   | 连续导出的名称。 该名称在数据库中必须独一无二，用于定期运行连续导出。      |
| ExternalTableName    | String   | 要导出到的[外部表](../externaltables.md)的名称。  |
| 查询                | String   | 要导出的查询。  |
| over (T1, T2)        | String   | 查询中可选的以逗号分隔的事实数据表列表。 如果不指定此项，将假定查询中引用的所有表都是事实数据表。 如果指定此项，则不在此列表中的表将被视为维度表，并且将没有作用域（所有记录都将参与所有导出）。 有关详细信息，请参阅[“说明”部分](#notes)。 |
| intervalBetweenRuns  | Timespan | 连续导出执行之间的时间跨度。 必须大于 1 分钟。   |
| forcedLatency        | Timespan | 一个可选的时间段，将查询范围限定为在此时间段之前（相对于当前时间）引入的记录。 例如，如果查询执行一些聚合/联接操作，而你想要确保在运行导出之前已引入所有相关记录，则此属性很有用。

除此之外，连续导出的创建命令支持[导出到外部表命令](export-data-to-an-external-table.md)支持的所有属性。 

**示例：**

```kusto
.create-or-alter continuous-export MyExport
over (T)
to table ExternalBlob
with
(intervalBetweenRuns=1h, 
 forcedLatency=10m, 
 sizeLimit=104857600)
<| T
```

| 名称     | ExternalTableName | 查询 | ForcedLatency | IntervalBetweenRuns | CursorScopedTables         | ExportProperties                   |
|----------|-------------------|-------|---------------|---------------------|----------------------------|------------------------------------|
| MyExport | ExternalBlob      | S     | 00:10:00      | 01:00:00            | [<br>  "['DB'].['S']"<br>] | {<br>  "SizeLimit":104857600<br>} |

## <a name="show-continuous-export"></a>显示连续导出

**语法：**

`.show` `continuous-export` *ContinuousExportName*

返回 ContinuousExportName 的连续导出属性。 

**属性：**

| 属性             | 类型   | 说明                |
|----------------------|--------|----------------------------|
| ContinuousExportName | String | 连续导出的名称。 |


`.show` `continuous-exports`

返回数据库中的所有连续导出。 

**输出：**

| 输出参数    | 类型     | 说明                                                             |
|---------------------|----------|-------------------------------------------------------------------------|
| CursorScopedTables  | String   | 显式限定了作用域的（事实数据）表的列表（序列化的 JSON）               |
| ExportProperties    | String   | 导出属性（序列化的 JSON）                                     |
| ExportedTo          | DateTime | 上次成功导出的日期时间（引入时间）       |
| ExternalTableName   | String   | 外部表的名称                                              |
| ForcedLatency       | TimeSpan | 强制延迟（如果未提供，则为 null）                                   |
| IntervalBetweenRuns | TimeSpan | 两次运行之间的间隔                                                   |
| IsDisabled          | 布尔  | 如果连续导出已禁用，则为 true                               |
| IsRunning           | 布尔  | 如果连续导出当前正在运行，则为 true                      |
| LastRunResult       | String   | 上次连续导出运行的结果（`Completed` 或 `Failed`） |
| LastRunTime         | DateTime | 上次执行连续导出的时间（开始时间）           |
| 名称                | String   | 连续导出的名称                                           |
| 查询               | String   | 导出查询                                                            |
| StartCursor         | String   | 此连续导出的首次执行的起点         |

## <a name="show-continuous-export-artifacts"></a>显示连续导出项目

**语法：**

`.show` `continuous-export` *ContinuousExportName* `exported-artifacts`

返回连续导出在所有运行中导出的所有项目。 可以在命令中按 Timestamp 列筛选结果，以仅查看感兴趣的记录。 导出的项目的历史记录将保留 14 天。 

**属性：**

| 属性             | 类型   | 说明                |
|----------------------|--------|----------------------------|
| ContinuousExportName | String | 连续导出的名称。 |

**输出：**

| 输出参数  | 类型     | 说明                            |
|-------------------|----------|----------------------------------------|
| Timestamp         | datetime | 连续导出运行的时间戳 |
| ExternalTableName | String   | 外部表的名称             |
| `Path`              | String   | 输出路径                            |
| NumRecords        | long     | 导出到路径的记录数     |

**示例：** 

```kusto
.show continuous-export MyExport exported-artifacts | where Timestamp > ago(1h)
```

| Timestamp                   | ExternalTableName | `Path`             | NumRecords | SizeInBytes |
|-----------------------------|-------------------|------------------|------------|-------------|
| 2018-12-20 07:31:30.2634216 | ExternalBlob      | `http://storageaccount.blob.core.chinacloudapi.cn/container1/1_6ca073fd4c8740ec9a2f574eaa98f579.csv` | 10 个                          | 1024              |

## <a name="show-continuous-export-failures"></a>显示连续导出失败

**语法：**

`.show` `continuous-export` *ContinuousExportName* `failures`

返回已记录为连续导出的一部分的所有失败。 可以在命令中按 Timestamp 列筛选结果，以仅查看感兴趣的时间范围。 

**属性：**

| 属性             | 类型   | 说明                |
|----------------------|--------|----------------------------|
| ContinuousExportName | String | 连续导出的名称  |

**输出：**

| 输出参数 | 类型      | 说明                                         |
|------------------|-----------|-----------------------------------------------------|
| Timestamp        | datetime  | 失败时的时间戳。                           |
| OperationId      | String    | 失败的操作 ID。                    |
| 名称             | String    | 连续导出名称。                             |
| LastSuccessRun   | Timestamp | 连续导出的上次成功运行。   |
| FailureKind      | String    | Failure/PartialFailure。 PartialFailure 表示在失败之前已成功导出某些项目。 |
| 详细信息          | String    | 失败错误详细信息。                              |

**示例：** 

```kusto
.show continuous-export MyExport failures 
```

| Timestamp                   | OperationId                          | 名称     | LastSuccessRun              | FailureKind | 详细信息    |
|-----------------------------|--------------------------------------|----------|-----------------------------|-------------|------------|
| 2019-01-01 11:07:41.1887304 | ec641435-2505-4532-ba19-d6ab88c96a9d | MyExport | 2019-01-01 11:06:35.6308140 | 失败     | 详细信息... |

## <a name="drop-continuous-export"></a>删除连续导出

**语法：**

`.drop` `continuous-export` *ContinuousExportName*

**属性：**

| 属性             | 类型   | 说明                |
|----------------------|--------|----------------------------|
| ContinuousExportName | String | 连续导出的名称 |

**输出：**

数据库中的其余连续导出（删除后）。 输出架构，与[显示连续导出命令](#show-continuous-export)中显示的相同。

## <a name="disable-or-enable-continuous-export"></a>禁用或启用连续导出

**语法：**

`.enable` `continuous-export` *ContinuousExportName* 

`.disable` `continuous-export` *ContinuousExportName* 

你可以禁用或启用连续导出作业。 不会执行禁用的连续导出，但其当前状态将保留，并且可以在启用连续导出后恢复。 当启用已长时间禁用的连续导出时，导出将从上次禁用导出时停止的位置继续进行。 如果没有足够的群集容量来为所有进程提供服务，此继续操作可能会导致导出长时间运行，阻止其他导出运行。 连续导出将按上次运行时间升序执行（最早的导出将首先运行，直到赶上进度）。 

**属性：**

| 属性             | 类型   | 说明                |
|----------------------|--------|----------------------------|
| ContinuousExportName | String | 连续导出的名称 |

**输出：**

对更改后的连续导出执行[显示连续导出命令](#show-continuous-export)的结果。 




## <a name="exporting-historical-data"></a>导出历史数据

连续导出只从其创建时间点开始导出数据。 应该使用（非连续）[导出命令](export-data-to-an-external-table.md)单独导出在该时间之前引入的记录。 为了避免与连续导出所导出的数据重复，请使用[显示连续导出命令](#show-continuous-export)返回的 StartCursor，并只导出 cursor_before_or_at 该游标值的记录。 请参阅以下示例。 历史数据可能太大，无法在单个导出命令中导出。 因此，请将查询分成几个较小的批。 

```kusto
.show continuous-export MyExport | project StartCursor
```

| StartCursor        |
|--------------------|
| 636751928823156645 |

后面是： 

```kusto
.export async to table ExternalBlob
<| T | where cursor_before_or_at("636751928823156645")
```
