---
title: hll()（聚合函数）- Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 hll()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 01/15/2020
ms.date: 08/06/2020
ms.openlocfilehash: 1a398acbad0c28e05b969f99fb81dddeb0f3ec85
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841441"
---
# <a name="hll-aggregation-function"></a>hll()（聚合函数）

计算整个组中 [`dcount`](dcount-aggfunction.md) 的中间结果，仅在 [summarize](summarizeoperator.md) 内的聚合上下文中计算。

阅读[基础算法 (HyperLogLog  ) 和估算准确度](dcount-aggfunction.md#estimation-accuracy)。

**语法**

`summarize hll(`*`Expr`* `[,` *`Accuracy`*`])`

**参数**

* *`Expr`* ：用于聚合计算的表达式。 
* 如果指定， *`Accuracy`* 可控制速度和准确度之间的平衡。

  |准确度值 |精确度  |Speed  |错误  |
  |---------|---------|---------|---------|
  |`0` | lowest | 最快 | 1.6% |
  |`1` | 默认值  | 均衡 | 0.8% |
  |`2` | high | slow | 0.4%  |
  |`3` | high | slow | 0.28% |
  |`4` | 超高 | 最慢 | 0.2% |
    
**返回**

在整个组中对 *`Expr`* 进行非重复计数的中间结果。
 
**提示**

1. 可以使用聚合函数 [`hll_merge`](hll-merge-aggfunction.md) 来合并多个 `hll` 中间结果（仅处理 `hll` 输出）。

1. 可以使用函数 [`dcount_hll`](dcount-hllfunction.md)，该函数将根据 `hll` / `hll_merge` 聚合函数计算 `dcount`。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents
| summarize hll(DamageProperty) by bin(StartTime,10m)

```

|StartTime|`hll_DamageProperty`|
|---|---|
|2007-09-18 20:00:00.0000000|[[1024,14],[-5473486921211236216,-6230876016761372746,3953448761157777955,4246796580750024372],[]]|
|2007-09-20 21:50:00.0000000|[[1024,14],[4835649640695509390],[]]|
|2007-09-29 08:10:00.0000000|[[1024,14],[4246796580750024372],[]]|
|2007-12-30 16:00:00.0000000|[[1024,14],[4246796580750024372,-8936707700542868125],[]]|
