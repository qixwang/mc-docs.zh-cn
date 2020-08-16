---
title: sumif()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 sumif()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 267060343a38b1747ac934b792be16ac8efdd8f0
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841641"
---
# <a name="sumif-aggregation-function"></a>sumif()（聚合函数）

返回谓词计算结果为 `true` 的“Expr”的总和。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

还可以使用 [sum()](sum-aggfunction.md) 函数，该函数在没有谓词表达式的情况下对行求和。

**语法**

summarize `sumif(`*Expr*`,`*Predicate*`)`

**参数**

* Expr：用于聚合计算的表达式。 
* Predicate：谓词，如果为 true，则 Expr 的计算值将添加到总和。 

**返回**

返回谓词计算结果为 `true` 的 Expr 总和值。

**示例**

```kusto
let T = datatable(name:string, day_of_birth:long)
[
   "John", 9,
   "Paul", 18,
   "George", 25,
   "Ringo", 7
];
T
| summarize sumif(day_of_birth, strlen(name) > 4)
```

|sumif_day_of_birth|
|----|
|32|