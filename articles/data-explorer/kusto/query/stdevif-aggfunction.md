---
title: stdevif()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 stdevif()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 84d3d92a064cb7ec8b5c6423dac59ce748d71f41
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841708"
---
# <a name="stdevif-aggregation-function"></a>stdevif()（聚合函数）

计算其谓词的计算结果为 `true` 的组内 Expr 的[stdev](stdev-aggfunction.md) 。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

summarize `stdevif(`*Expr*`, `*Predicate*`)`

**参数**

* Expr：用于聚合计算的表达式。 
* Predicate：谓词，如果为 true，则 Expr 计算值将添加到标准偏差中。

**返回**

其谓词计算结果为 `true` 的组内 Expr 的标准偏差值 。
 
**示例**

```kusto
range x from 1 to 100 step 1
| summarize stdevif(x, x%2 == 0)

```

|stdevif_x|
|---|
|29.1547594742265|