---
title: make_list_if()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 make_list_if()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 334b1bdd48c4da6f37c012ea8173b7af69204c83
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841192"
---
# <a name="make_list_if-aggregation-function"></a>make_list_if()（聚合函数）

返回组中“Expr”的所有值的 `dynamic` (JSON) 数组，其 Predicate 的计算结果为 `true`。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

`summarize` `make_list_if(`*Expr*, *Predicate* [`,` *MaxSize*]`)`

**参数**

* Expr：用于聚合计算的表达式。
* *谓词*：必须计算为 `true` 的谓词，用于将“Expr”添加到结果中。
* MaxSize 是对返回元素最大数目的可选整数限制（默认值是 1048576）。 MaxSize 值不能超过 1048576。

**返回**

返回组中“Expr”的所有值的 `dynamic` (JSON) 数组，其 Predicate 的计算结果为 `true`。
如果未对 `summarize` 运算符的输入进行排序，那么生成的数组中的元素顺序是不确定的。
如果对 `summarize` 运算符的输入进行了排序，则生成的数组中的元素顺序和输入一样。

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
| summarize make_list_if(name, strlen(name) > 4)
```

|list_name|
|----|
|["George", "Ringo"]|

**另请参阅**

[`make_list`](./makelist-aggfunction.md) 函数，它在无谓词表达式的情况下执行相同的操作。