---
title: stdev()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 stdev()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 251ebe29cf94be6e5a6f23d8337f50500746335a
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841709"
---
# <a name="stdev-aggregation-function"></a>stdev()（聚合函数）

将该组作为示例，计算组内 Expr 的标准偏差。 

* 使用的公式：

:::image type="content" source="images/stdev-aggfunction/stdev-sample.png" alt-text="Stdev 示例":::

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

summarize `stdev(`Expr`)`

**参数**

* Expr：用于聚合计算的表达式。 

**返回**

组内 Expr 的标准偏差值。
 
**示例**

```kusto
range x from 1 to 5 step 1
| summarize make_list(x), stdev(x)

```

|list_x|stdev_x|
|---|---|
|[ 1, 2, 3, 4, 5]|1.58113883008419|