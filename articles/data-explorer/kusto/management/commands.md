---
title: 命令管理 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的命令管理。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: 7a284d86ff19d68b03aa9d27d4479699948e13de
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470435"
---
# <a name="commands-management"></a>命令管理

## <a name="show-commands"></a>.show 命令

`.show commands` 返回一个表，其中包含已达到最终状态的管理员命令。 这些命令可供查询 30 天。

命令表有两列，其中包含每个已完成命令的资源使用量详细信息。

- TotalCpu - 此命令使用的总 CPU 时钟时间（用户模式 + 内核模式）。
- ResourceUtilization - 包含与该命令相关的所有资源使用信息（包括 TotalCpu）。

所跟踪的资源使用量包括数据更新以及与当前的管理员命令关联的任何查询。
目前，命令表仅涵盖一些管理员命令（`.ingest`、`.set`、`.append`、`.set-or-replace`、`.set-or-append`）。 我们会逐渐将更多命令添加到命令表中。

- [数据库管理员或数据库监视员](../management/access-control/role-based-authorization.md)可以看到在其数据库上调用的任何命令。
- 其他用户只能看到由他们自己调用的命令。

**语法**

`.show` `commands`

**示例**

| ClientActivityId                                    | CommandType    | 文本                         | 数据库 | StartedOn                   | LastUpdatedOn               | 持续时间         | 状态     | RootActivityId                       | User                                  | FailureReason                            | 应用程序        | 主体                         | TotalCpu         | ResourceUtilization                                                                                                                                                                                                                                  |
| --------------------------------------------------- | -------------- | ---------------------------- | -------- | --------------------------- | --------------------------- | ---------------- | --------- | ------------------------------------ | ------------------------------------- | ---------------------------------------- | ------------------ | --------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| KD2RunCommand;a069f9e3-6062-4a0e-aa82-75a1b5e16fb4  | ExtentsMerge   | .merge async Operations ...  | DB1      | 2017-09-05 11:08:07.5738569 | 2017-09-05 11:08:09.1051161 | 00:00:01.5312592 | 已完成 | b965d809-3f3e-4f44-bd2b-5e1f49ac46c5 | AAD app id=5ba8cec2-9a70-e92c98cad651 |                                          | Kusto.Azure.DM.Svc | aadapp=5ba8cec2-9a70-e92c98cad651 | 00:00:03.5781250 | { "ScannedExtentsStatistics": { "MinDataScannedTime": null, "MaxDataScannedTime": null }, "CacheStatistics": { Memory": { "Misses":2, "Hits":20 }, "Disk": { "Misses":2, "Hits":0 } }, "MemoryPeak":159620640, "TotalCpu":"00:00:03.5781250" } |
| KE.RunCommand; 710e08ca-2cd3-4d2d-b7bd-2738d335aa50 | DataIngestPull | .ingest into MyTableName ... | TestDB   | 2017-09-04 16:00:37.0915452 | 2017-09-04 16:04:37.2834555 | 00:04:00.1919103 | 已失败    | a8986e9e-943f-81b0270d6fae4          | cooper@fabrikam.com                   | 套接字连接已释放。 | Kusto.Explorer     | aaduser=...                       | 00:00:00         | { "ScannedExtentsStatistics": { "MinDataScannedTime": null, "MaxDataScannedTime": null }, "CacheStatistics": { "Memory": { "Misses":0, Hits":0 }, "Disk": { "Misses":0, "Hits":0 } }, "MemoryPeak":0, "TotalCpu":"00:00:00"}                   |
| KD2RunCommand;97db47e6-93e2-4306-8b7d-670f2c3307ff  | ExtentsRebuild | .merge async Operations ...  | DB2      | 2017-09-18 13:29:38.5945531 | 2017-09-18 13:29:39.9451163 | 00:00:01.3505632 | 已完成 | d5ebb755-d5df-4e94-b240-9accdf06c2d1 | AAD app id=5ba8cec2-9a70-e92c98cad651 |                                          | Kusto.Azure.DM.Svc | aadapp=5ba8cec2-9a70-e92c98cad651 | 00:00:00.8906250 | { "ScannedExtentsStatistics": { "MinDataScannedTime": null, "MaxDataScannedTime": null }, "CacheStatistics": { Memory": { "Misses":0, "Hits":1 }, "Disk": { "Misses":0, "Hits":0 } }, "MemoryPeak":88828560, "TotalCpu":"00:00:00.8906250"}    |

**示例：从 ResourceUtilization 列中提取特定数据**

通过调用 ResourcesUtilization.xxx（其中的 xxx 是属性名称）来访问 ResourceUtilization 列中的属性之一。

> [!NOTE]
> `ResourceUtilization` 是动态列。 若要使用其值，应首先将其转换为特定的数据类型。 使用转换函数，例如 `tolong`、`toint`、`totimespan`。

例如，

```kusto
.show commands
| where CommandType == "TableAppend"
| where StartedOn > ago(1h)
| extend MemoryPeak = tolong(ResourcesUtilization.MemoryPeak)
| top 3 by MemoryPeak desc
| project StartedOn, MemoryPeak, TotalCpu, Text
```

| StartedOn                   | MemoryPeak | TotalCpu         | 文本                                                                                                                                         |
| --------------------------- | ---------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 2017-09-28 12:11:27.8155381 | 800396032  | 00:00:04.5312500 | .append Server_Boots <\| let bootStartsSourceTable = SessionStarts; ...                                                                      |
| 2017-09-28 11:21:26.7304547 | 750063056  | 00:00:03.8218750 | .set-or-append WebUsage <\| database('CuratedDB').WebUsage_v2                                                                                | summarize ... | project ... |
| 2017-09-28 12:16:17.4762522 | 676289120  | 00:00:00.0625000 | .set-or-append AtlasClusterEventStats with(...) <\| Atlas_Temp(datetime(2017-09-28 12:13:28.7621737), datetime(2017-09-28 12:14:28.8168492)) |

## <a name="investigating-performance-issues"></a>调查性能问题

`.show` `commands` 可用于调查性能问题，因为这些命令会显示每个控制命令占用的资源。

以下示例是用于此类调查的简单而有用的查询。

### <a name="query-the-totalcpu-column"></a>查询 TotalCpu 列

在过去一天内消耗 CPU 最多的前 10 个查询。

```kusto
.show commands
| where StartedOn > ago(1d)
| top 10 by TotalCpu
| project StartedOn, CommandType, ClientActivityId, TotalCpu
```

在过去 10 小时内 TotalCpu 超过 3 分钟的所有查询。

```kusto
.show commands
| where StartedOn > ago(10h) and TotalCpu > 3m
| project StartedOn, CommandType, ClientActivityId, TotalCpu
| order by TotalCpu
```

在过去 24 小时内 TotalCpu 超过 5 分钟的所有查询（按用户和主体分组）。

```kusto
.show commands
| where StartedOn > ago(24h)
| summarize TotalCount=count(), CountAboveThreshold=countif(TotalCpu > 2m), AverageCpu = avg(TotalCpu), MaxTotalCpu=max(TotalCpu), (50th_Percentile_TotalCpu, 95th_Percentile_TotalCpu)=percentiles(TotalCpu, 50, 95) by User, Principal
| extend PercentageAboveThreshold = strcat(substring(CountAboveThreshold * 100 / TotalCount, 0, 5), "%")
| order by CountAboveThreshold desc
| project User, Principal, CountAboveThreshold, TotalCount, PercentageAboveThreshold, MaxTotalCpu, AverageCpu, 50th_Percentile_TotalCpu, 95th_Percentile_TotalCpu
```

时间表：平均 CPU 与第 95 个百分点值与最大 CPU。

```kusto
.show commands
| where StartedOn > ago(1d)
| summarize MaxCpu_Minutes=max(TotalCpu)/1m, 95th_Percentile_TotalCpu_Minutes=percentile(TotalCpu, 95)/1m, AverageCpu_Minutes=avg(TotalCpu)/1m by bin(StartedOn, 1m)
| render timechart
```

## <a name="query-the-memorypeak"></a>查询 MemoryPeak

在过去一天内 MemoryPeak 值最高的前 10 个查询。

```kusto
.show commands
| where StartedOn > ago(1d)
| extend MemoryPeak = tolong(ResourcesUtilization.MemoryPeak)
| project StartedOn, CommandType, ClientActivityId, TotalCpu, MemoryPeak
| top 10 by MemoryPeak
```

平均 MemoryPeak 与第 95 个百分点值与最大 MemoryPeak 的时间表。

```kusto
.show commands
| where StartedOn > ago(1d)
| project MemoryPeak = tolong(ResourcesUtilization.MemoryPeak), StartedOn
| summarize Max_MemoryPeak=max(MemoryPeak), 95th_Percentile_MemoryPeak=percentile(MemoryPeak, 95), Average_MemoryPeak=avg(MemoryPeak) by bin(StartedOn, 1m)
| render timechart
```
