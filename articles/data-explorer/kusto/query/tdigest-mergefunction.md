---
title: tdigest_merge() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 tdigest_merge()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 12/09/2019
ms.date: 08/06/2020
ms.openlocfilehash: 89a287b2e93adec0e3596a9f21a3b0fbca49080f
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841546"
---
# <a name="tdigest_merge"></a>tdigest_merge()

合并 `tdigest` 结果（聚合版 [`tdigest_merge()`](tdigest-merge-aggfunction.md) 的标量版本）。

在[此处](percentiles-aggfunction.md#estimation-error-in-percentiles)详细了解基础算法 (T-Digest) 和预估误差。

**语法**

`merge_tdigests(` *Expr1*`,` *Expr2*`, ...)`

`tdigest_merge(` *Expr1*`,` *Expr2*`, ...)` - 别名。

**参数**

* 列，包含要合并的 `tdigest` 值。

**返回**

将列 `*Expr1*`、`*Expr2*`、... `*ExprN*` 合并为一个 `tdigest` 后的结果。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 10 step 1 
| extend y = x + 10
| summarize tdigestX = tdigest(x), tdigestY = tdigest(y)
| project merged = tdigest_merge(tdigestX, tdigestY)
| project percentile_tdigest(merged, 100, typeof(long))
```

|percentile_tdigest_merged|
|---|
|20|
