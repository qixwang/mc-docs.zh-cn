---
title: dcount_hll() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 dcount_hll()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 04/15/2019
ms.date: 08/06/2020
ms.openlocfilehash: d080b5dd2de835476c834ec6ba7c990272a5ddf1
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841540"
---
# <a name="dcount_hll"></a>dcount_hll()

根据 hll 结果（已由 [hll](hll-aggfunction.md) 或 [hll_merge](hll-merge-aggfunction.md) 生成）计算 dcount。

阅读[基础算法 (HyperLogLog  ) 和估算准确度](dcount-aggfunction.md#estimation-accuracy)。

**语法**

`dcount_hll(`*Expr*`)`

**参数**

* Expr：已由 [hll](hll-aggfunction.md) 或 [hll-merge](hll-merge-aggfunction.md) 生成的表达式

**返回**

Expr 中每个值的非重复计数

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents
| summarize hllRes = hll(DamageProperty) by bin(StartTime,10m)
| summarize hllMerged = hll_merge(hllRes)
| project dcount_hll(hllMerged)
```

|dcount_hll_hllMerged|
|---|
|315|
