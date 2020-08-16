---
title: hll_merge()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 hll_merge()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 04/15/2019
ms.date: 08/06/2020
ms.openlocfilehash: 1f1a8c43db0853604303f82da31643eca8cdd5e5
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841398"
---
# <a name="hll_merge-aggregation-function"></a>hll_merge()（聚合函数）

将组中的 `HLL` 结果合并为单个 `HLL` 值。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用。

有关详细信息，请参阅[基础算法 (HyperLogLog  ) 和估算准确度](dcount-aggfunction.md#estimation-accuracy)。

**语法**

`summarize` `hll_merge(`*Expr*`)`

**参数**

* `*Expr*`：将要用于聚合计算的表达式。

**返回**

此函数返回组中 `*Expr*` 的合并 `hll` 值。
 
**提示**

1) 使用函数 [dcount_hll] (dcount-hllfunction.md) 根据 `hll` / `hll-merge` 聚合函数计算 `dcount`。
