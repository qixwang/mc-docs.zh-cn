---
title: make_set_if()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 make_set_if()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: db1dc789c861478e5e4afe3aae4a3841bd2159fd
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841194"
---
# <a name="make_set_if-aggregation-function"></a>make_set_if()（聚合函数）

返回一个 `dynamic` (JSON) 数组，该数组包含 Expr 在组中接受的非重复值集，其 Predicate 的计算结果为 `true`。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

`summarize` `make_set_if(`*Expr*, *Predicate* [`,` *MaxSize*]`)`

**参数**

* Expr：用于聚合计算的表达式。
* *谓词*：必须计算为 `true` 的谓词，用于将“Expr”添加到结果中。
* MaxSize 是对返回元素最大数目的可选整数限制（默认值是 1048576）。 MaxSize 值不能超过 1048576。

**返回**

返回一个 `dynamic` (JSON) 数组，该数组包含 Expr 在组中接受的非重复值集，其 Predicate 的计算结果为 `true`。
数组的排序顺序未定义。

> [!TIP]
> 若要仅对非重复值进行计数，请使用 [dcountif()](dcountif-aggfunction.md)

**另请参阅**

[`make_set`](./makeset-aggfunction.md) 函数，它在无谓词表达式的情况下执行相同的操作。

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
| summarize make_set_if(name, strlen(name) > 4)
```

|set_name|
|----|
|["George", "Ringo"]|