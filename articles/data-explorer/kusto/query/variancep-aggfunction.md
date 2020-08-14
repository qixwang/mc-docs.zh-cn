---
title: variancep()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 variancep()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 8aa7b0c6135e0df2caf225035bfbdf44d21e5dbf
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841327"
---
# <a name="variancep-aggregation-function"></a>variancep()（聚合函数）

将某组视为总体，计算该组内 Expr 的方差。 

* 使用的公式：

:::image type="content" source="images/variancep-aggfunction/variance-population.png" alt-text="方差总体":::

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

summarize `variancep(`Expr`)`

**参数**

* Expr：用于聚合计算的表达式。 

**返回**

组中 Expr 的方差值。
 
**示例**

```kusto
range x from 1 to 5 step 1
| summarize make_list(x), variancep(x) 
```

|list_x|variance_x|
|---|---|
|[ 1, 2, 3, 4, 5]|2|