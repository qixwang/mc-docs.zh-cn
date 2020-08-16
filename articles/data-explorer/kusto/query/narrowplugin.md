---
title: narrow 插件 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 narrow 插件。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 18890fb0c9d5ddfab40904cc31b4d8fd5ef15260
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841195"
---
# <a name="narrow-plugin"></a>narrow 插件

```kusto
T | evaluate narrow()
```

`narrow` 插件将宽表“逆透视”为只包含三列的表：行号、列类型和列值（作为 `string`）。

`narrow` 插件主要用于显示目的，它可以让你在不需要水平滚动的情况下轻松显示宽表。

**语法**

`T | evaluate narrow()`

**示例**

以下示例演示了如何使用简单的方法来读取 Kusto 的 `.show diagnostics` 控制命令的输出。

```kusto
.show diagnostics
 | evaluate narrow()
```

`.show diagnostics` 本身的结果是一个具有单个行和 33 个列的表。 通过使用 `narrow` 插件，我们将输出“旋转”为如下所示的结果：

行  | 列                              | “值”
-----|-------------------------------------|-----------------------------
0    | IsHealthy                           | True
0    | IsRebalanceRequired                 | 错误
0    | IsScaleOutRequired                  | 错误
0    | MachinesTotal                       | 2
0    | MachinesOffline                     | 0
0    | NodeLastRestartedOn                 | 2017-03-14 10:59:18.9263023
0    | AdminLastElectedOn                  | 2017-03-14 10:58:41.6741934
0    | ClusterWarmDataCapacityFactor       | 0.130552847673333
0    | ExtentsTotal                        | 136
0    | DiskColdAllocationPercentage        | 5
0    | InstancesTargetBasedOnDataCapacity  | 2
0    | TotalOriginalDataSize               | 5167628070
0    | TotalExtentSize                     | 1779165230
0    | IngestionsLoadFactor                | 0
0    | IngestionsInProgress                | 0
0    | IngestionsSuccessRate               | 100
0    | MergesInProgress                    | 0
0    | BuildVersion                        | 1.0.6281.19882
0    | BuildTime                           | 2017-03-13 11:02:44.0000000
0    | ClusterDataCapacityFactor           | 0.130552847673333
0    | IsDataWarmingRequired               | 错误
0    | RebalanceLastRunOn                  | 2017-03-21 09:14:53.8523455
0    | DataWarmingLastRunOn                | 2017-03-21 09:19:54.1438800
0    | MergesSuccessRate                   | 100
0    | NotHealthyReason                    | [null]
0    | IsAttentionRequired                 | 错误
0    | AttentionRequiredReason             | [null]
0    | ProductVersion                      | KustoRelease_2017.03.13.2
0    | FailedIngestOperations              | 0
0    | FailedMergeOperations               | 0
0    | MaxExtentsInSingleTable             | 64
0    | TableWithMaxExtents                 | KustoMonitoringPersistentDatabase.KustoMonitoringTable
0    | WarmExtentSize                      | 1779165230