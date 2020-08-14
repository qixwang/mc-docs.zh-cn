---
title: stdevp()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 stdevp()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: e7cadddadc46d1702a9a059b09057fd642ad0131
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841707"
---
# <a name="stdevp-aggregation-function"></a>stdevp()（聚合函数）

将组视为一个总体，计算整个组中 Expr 的标准偏差。 

* 使用的公式：

:::image type="content" source="images/stdevp-aggfunction/stdev-population.png" alt-text="Stdev 总体":::

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

summarize `stdevp(`Expr`)`

**参数**

* Expr：用于聚合计算的表达式。 

**返回**

整个组内 Expr 的标准偏差值。
 
**示例**

```kusto
range x from 1 to 5 step 1
| summarize make_list(x), stdevp(x)

```

|list_x|stdevp_x|
|---|---|
|[ 1, 2, 3, 4, 5]|1.4142135623731|