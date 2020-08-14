---
title: make_list_with_nulls()（聚合函数） - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 make_list_with_nulls()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/09/2020
ms.date: 08/06/2020
ms.openlocfilehash: 9c8afe600a238a771cc5076bad42eb2ec853e41e
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841371"
---
# <a name="make_list_with_nulls-aggregation-function"></a>make_list_with_nulls()（聚合函数）

返回组中 Expr 的所有值的 `dynamic` (JSON) 数组（包括 null 值）。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

`summarize` `make_list_with_nulls(` *Expr* `)`

**参数**

* Expr：用于聚合计算的表达式。

**返回**

返回组中 Expr 的所有值的 `dynamic` (JSON) 数组（包括 null 值）。
如果未对 `summarize` 运算符的输入进行排序，则生成的数组中的元素顺序是不确定的。
如果对 `summarize` 运算符的输入进行了排序，则生成的数组中的元素顺序和输入一样。

> [!TIP]
> 使用 [`mv-apply`](./mv-applyoperator.md) 运算符按某个键值创建一个有序列表。 请参阅[此处的](./mv-applyoperator.md#using-the-mv-apply-operator-to-sort-the-output-of-makelist-aggregate-by-some-key)示例。
