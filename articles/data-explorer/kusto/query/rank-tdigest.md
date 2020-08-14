---
title: rank_tdigest() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 rank_tdigest()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 12/10/2019
ms.date: 08/06/2020
ms.openlocfilehash: 102d78477fea4ce33140a5969ef90b84b2a97e37
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841511"
---
# <a name="rank_tdigest"></a>rank_tdigest()

计算数据集中值的近似排序。 数据集 `S` 中值 `v` 的排序定义为 `S` 中小于或等于 `v` 的成员的计数，`S` 使用其 `tdigest` 表示。

**语法**

`rank_tdigest(`*`TDigest`*`,` *`Expr`*`)`

**参数**

* TDigest：[tdigest()](tdigest-aggfunction.md) 或 [tdigest_merge()](tdigest-merge-aggfunction.md) 生成的表达式
* Expr：表示要用于排名计算的值的表达式。

**返回**

数据集中每个值的排名。

**提示**

1) 要获取其排名的值必须与 `tdigest` 为同一类型。

**示例**

在排序列表 (1-1000) 中，排名 685 是其索引：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 1000 step 1
| summarize t_x=tdigest(x)
| project rank_of_685=rank_tdigest(t_x, 685)
```

|`rank_of_685`|
|-------------|
|`685`        |

此查询计算所有损坏财产成本的值 4490$ 的排名：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents
| summarize tdigestRes = tdigest(DamageProperty)
| project rank_of_4490=rank_tdigest(tdigestRes, 4490) 

```

|`rank_of_4490`|
|--------------|
|`50207`       |

获取排名的估计百分比（通过除以数据集大小）：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents
| summarize tdigestRes = tdigest(DamageProperty), count()
| project rank_tdigest(tdigestRes, 4490) * 100.0 / count_

```

|`Column1`         |
|------------------|
|`85.0015237192293`|


损坏财产成本的第 85 个百分位数为 4490$：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents
| summarize tdigestRes = tdigest(DamageProperty)
| project percentile_tdigest(tdigestRes, 85, typeof(long))

```

|`percentile_tdigest_tdigestRes`|
|-------------------------------|
|`4490`                         |


