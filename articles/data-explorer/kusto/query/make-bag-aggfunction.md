---
title: make_bag()（聚合函数）- Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 make_bag() 聚合函数。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 15f623251d0c6f5748abd4abbee496ce1d089052
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841292"
---
# <a name="make_bag-aggregation-function"></a>make_bag()（聚合函数）

返回组中 `Expr` 的所有值的 `dynamic` (JSON) 属性包（字典）。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

`summarize` `make_bag(`*`Expr`* [`,` *MaxSize*]`)`

**参数**

* Expr：用于聚合计算的 `dynamic` 类型的表达式。
* MaxSize 是对返回元素最大数目的可选整数限制。 默认值为 1048576。 MaxSize 值不能超过 1048576。

**注意**

函数 `make_dictionary()` 的旧版和已过时变体的默认限制为 MaxSize = 128。

**返回**

返回组（属性包）中 `Expr` 的所有值的 `dynamic` (JSON) 属性包（字典）。
将跳过非字典值。
如果一个键出现在多个行中，则会从此键的可能值中选择一个任意值。

**另请参阅**

使用 [bag_unpack()](bag-unpackplugin.md) 插件将动态 JSON 对象扩展到使用属性包键的列中。 

**示例**

```kusto
let T = datatable(prop:string, value:string)
[
    "prop01", "val_a",
    "prop02", "val_b",
    "prop03", "val_c",
];
T
| extend p = pack(prop, value)
| summarize dict=make_bag(p)

```

|dict|
|----|
|{ "prop01": "val_a", "prop02": "val_b", "prop03": "val_c" } |

使用 [bag_unpack()](bag-unpackplugin.md) 插件将 make_bag() 输出中的包键转换为列。 

```kusto
let T = datatable(prop:string, value:string)
[
    "prop01", "val_a",
    "prop02", "val_b",
    "prop03", "val_c",
];
T
| extend p = pack(prop, value)
| summarize bag=make_bag(p)
| evaluate bag_unpack(bag) 

```

|prop01|prop02|prop03|
|---|---|---|
|val_a|val_b|val_c|
