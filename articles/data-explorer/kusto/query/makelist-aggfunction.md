---
title: make_list()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 make_list()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 01/23/2020
ms.date: 08/06/2020
ms.openlocfilehash: bc563d659a9332c5e9d86edb9f342d62150e0917
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841622"
---
# <a name="make_list-aggregation-function"></a>make_list()（聚合函数）

返回组中 *Expr* 所有值的 `dynamic` (JSON) 数组。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

`summarize` `make_list(`*Expr* [`,` *MaxSize*]`)`

**参数**

* Expr：用于聚合计算的表达式。
* MaxSize 是对返回元素最大数目的可选整数限制（默认值是 1048576）。 MaxSize 值不能超过 1048576。

> [!NOTE]
> 函数 `makelist()` 的旧版和已过时变体的默认限制为 MaxSize = 128。

**返回**

返回组中 *Expr* 所有值的 `dynamic` (JSON) 数组。
如果未对 `summarize` 运算符的输入进行排序，那么生成的数组中的元素顺序是不确定的。
如果对 `summarize` 运算符的输入进行了排序，则生成的数组中的元素顺序和输入一样。

> [!TIP]
> 使用 [`mv-apply`](./mv-applyoperator.md) 运算符按某个键值创建一个有序列表。 请参阅[此处的](./mv-applyoperator.md#using-the-mv-apply-operator-to-sort-the-output-of-makelist-aggregate-by-some-key)示例。

**另请参阅**

[`make_list_if`](./makelistif-aggfunction.md) 运算符与 `make_list` 相似，只是它也接受谓词。