---
title: sort 运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 sort 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 467ffe58f699c0934448c9e0c75b943e72c66b2f
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841566"
---
# <a name="sort-operator"></a>sort 运算符 

按照一个或多个列的顺序对输入表的行排序。

```kusto
T | sort by strlen(country) asc, price desc
```

**Alias**

`order`

**语法**

*T* `| sort by` *expression* [`asc` | `desc`] [`nulls first` | `nulls last`] [`,` ...]

**参数**

* *T*：要排序的表输入。
* expression：要作为排序依据的标量表达式。 值的类型必须是数字、日期、时间或字符串。
* `asc` 按升序（即由低到高）排列。 默认值是 `desc`，降序，由高到低。
* `nulls first`（`asc` 顺序的默认值）将把 null 值放在开头，`nulls last`（`desc` 顺序的默认值）将把 null 值放在末尾。

**示例**

```kusto
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc nulls first
```

表 Traces 中具有特定 `ActivityId` 的所有行，按时间戳排序。 如果 `Timestamp` 列包含 null 值，则这些值将显示在结果的前几行。

为了从结果中排除 null 值，请在调用排序运算符之前添加筛选器：

```kusto
Traces
| where ActivityId == "479671d99b7b" and isnotnull(Timestamp)
| sort by Timestamp asc
```