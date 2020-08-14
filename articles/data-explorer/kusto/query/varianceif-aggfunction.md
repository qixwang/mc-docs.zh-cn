---
title: varianceif()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 varianceif()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 15510d7b2ef2e031de755debcbda4bcc72a711c7
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841328"
---
# <a name="varianceif-aggregation-function"></a>Varianceif()（聚合函数）

计算“谓词”求值为 `true` 的组内的“Expr”的[变型](variance-aggfunction.md) 。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

summarize `varianceif(`*Expr*`, `*Predicate*`)`

**参数**

* Expr：用于聚合计算的表达式。 
* “谓词”：谓词如果为 true，则 Expr 计算值随即添加到变型中 。

**返回**

谓词计算结果为 `true` 的组内 Expr 的变型值 。
 
**示例**

```kusto
range x from 1 to 100 step 1
| summarize varianceif(x, x%2 == 0)

```

|varianceif_x|
|---|
|850|