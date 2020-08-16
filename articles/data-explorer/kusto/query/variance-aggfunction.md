---
title: variance()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 variance()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 28f0593af2be021dfc05bd9fdd348ece22763994
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841543"
---
# <a name="variance-aggregation-function"></a>variance()（聚合函数）

将某组视为样本，计算该组内 Expr 的方差。 

* 使用的公式：

:::image type="content" source="images/variance-aggfunction/variance-sample.png" alt-text="方差示例":::

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

summarize `variance(`Expr`)`

**参数**

* Expr：用于聚合计算的表达式。 

**返回**

组中 Expr 的方差值。
 
**示例**

```kusto
range x from 1 to 5 step 1
| summarize make_list(x), variance(x) 
```

|list_x|variance_x|
|---|---|
|[ 1, 2, 3, 4, 5]|2.5|