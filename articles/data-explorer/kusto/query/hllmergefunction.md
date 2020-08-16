---
title: hll_merge() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 hll_merge()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 04/15/2019
ms.date: 08/06/2020
ms.openlocfilehash: b4c13b6ddf0c84866513b8e7222f3ab6b7c3cb84
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841739"
---
# <a name="hll_merge"></a>hll_merge()

合并 `hll` 结果（聚合版 [`hll_merge()`](hll-merge-aggfunction.md) 的标量版本）。

阅读[基础算法 (HyperLogLog  ) 和估算准确度](dcount-aggfunction.md#estimation-accuracy)。

**语法**

`hll_merge(` *Expr1*`,` *Expr2*`, ...)`

**参数**

* 包含要合并的 `hll` 值的列。

**返回**

将列 `*Exrp1*`、`*Expr2*`、... `*ExprN*` 合并为一个 `hll` 值后的结果。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/KustoMonitoringPersistentDatabase -->
```kusto
range x from 1 to 10 step 1 
| extend y = x + 10
| summarize hll_x = hll(x), hll_y = hll(y)
| project merged = hll_merge(hll_x, hll_y)
| project dcount_hll(merged)
```

|`dcount_hll_merged`|
|---|
|20|
