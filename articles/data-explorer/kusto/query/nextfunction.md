---
title: next() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 next()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 2bc26ff0254913bcf44ccb12b581b6183a07db32
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841526"
---
# <a name="next"></a>next()

返回在[序列化行集](./windowsfunctions.md#serialized-row-set)中位于当前行之后的某个偏移量处的行中列的值。

**语法**

`next(column)`

`next(column, offset)`

`next(column, offset, default_value)`

**参数**

* `column`：要从中获取值的列。

* `offset`：行中向前的偏移量。 如果未指定偏移量，则使用默认偏移量 1。

* `default_value`：在没有要从中获取值的后续行时使用的默认值。 如果未指定默认值，则使用 null。


**示例**
```kusto
Table | serialize | extend nextA = next(A,1)
| extend diff = A - nextA
| where diff > 1

Table | serialize nextA = next(A,1,10)
| extend diff = A - nextA
| where diff <= 10
```