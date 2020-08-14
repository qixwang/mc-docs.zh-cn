---
title: iff() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 iff()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: f504f3baece190a1a68795dab463f02b6b5f89e3
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841737"
---
# <a name="iff"></a>iff()

计算第一个参数 (predicate) 并返回第二个或第三个参数的值，具体取决于 predicate 计算为 `true`（第二个）还是 `false`（第三个）。

第二个和第三个参数必须属于同一类型。

**语法**

`iff(`*predicate*`,` *ifTrue*`,` *ifFalse*`)`

**参数**

* predicate：一个计算结果为 `boolean` 值的表达式。
* ifTrue：如果 predicate 计算结果为 `true`，得以计算的表达式以及从函数返回的表达式值。
* ifFalse：如果 predicate 计算结果为 `false`，得以计算的表达式以及从函数返回的表达式值。

**返回**

如果 *predicate* 计算结果为 `true`，此函数返回 *ifTrue* 的值，否则返回 *ifFalse* 的值。

**示例**

```kusto
T 
| extend day = iff(floor(Timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```