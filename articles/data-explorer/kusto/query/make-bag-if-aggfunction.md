---
title: make_bag_if()（聚合函数）- Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 make_bag_if()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 91aee26284726d541d28fef8492602eb20dfc062
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841374"
---
# <a name="make_bag_if-aggregation-function"></a>make_bag_if()（聚合函数）

返回组中“Expr”的所有值的 `dynamic` (JSON) 属性包（字典），其 Predicate 的计算结果为 `true`。

> [!NOTE]
> 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用。

**语法**

`summarize` `make_bag_if(`*Expr*, *Predicate* [`,` *MaxSize*]`)`

**参数**

* Expr：将用于聚合计算的 `dynamic` 类型的表达式。
* *谓词*：必须计算为 `true` 的谓词，用于将“Expr”添加到结果中。
* MaxSize：对返回元素最大数目的可选整数限制（默认值是 1048576）。 MaxSize 值不能超过 1048576。

**返回**

返回组中“Expr”的所有值（属性包（字典））的 `dynamic` (JSON) 属性包（字典），其 Predicate 的计算结果为 `true`。
将跳过非字典值。
如果一个键出现在多个行中，则会从此键的可能值中选择一个任意值。

> [!NOTE]
> [`make_bag`](./make-bag-aggfunction.md) 函数类似于不带谓词表达式的 make_bag_if()。

**示例**

```kusto
let T = datatable(prop:string, value:string, predicate:bool)
[
    "prop01", "val_a", true,
    "prop02", "val_b", false,
    "prop03", "val_c", true
];
T
| extend p = pack(prop, value)
| summarize dict=make_bag_if(p, predicate)

```

|dict|
|----|
|{ "prop01": "val_a", "prop03": "val_c" } |

使用 [bag_unpack()](bag-unpackplugin.md) 插件将 make_bag_if() 输出中的包键转换为列。 

```kusto
let T = datatable(prop:string, value:string, predicate:bool)
[
    "prop01", "val_a", true,
    "prop02", "val_b", false,
    "prop03", "val_c", true
];
T
| extend p = pack(prop, value)
| summarize bag=make_bag_if(p, predicate)
| evaluate bag_unpack(bag)

```

|prop01|prop03|
|---|---|
|val_a|val_c|
