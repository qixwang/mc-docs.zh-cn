---
title: min()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 min()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 07/24/2019
ms.date: 08/06/2020
ms.openlocfilehash: 892ce8929a5233eb3859e12286dd8a6d8a238c5d
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841435"
---
# <a name="min-aggregation-function"></a>min()（聚合函数）

返回组内的最小值。 

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

`summarize` `min(`*Expr*`)`

**参数**

* Expr：用于聚合计算的表达式。 

**返回**

组内 Expr 的最小值。
 
> [!TIP]
> 这能提供该项本身的最大值或最小值 - 例如最高或最低价格。 但如果需要行中的其他列 - 例如，提供最低价格的供应商的名称 - 则需使用 [arg_max](arg-max-aggfunction.md) 或 [arg_min](arg-min-aggfunction.md)。