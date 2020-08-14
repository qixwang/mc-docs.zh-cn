---
title: iif() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 iif()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 2da172aa3d7cf0a3076c9ae095a9db87b3909ebc
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841735"
---
# <a name="iif"></a>iif()

计算第一个参数 (predicate) 并返回第二个或第三个参数的值，具体取决于 predicate 计算为 `true`（第二个）还是 `false`第三个）。

第二个和第三个参数必须属于同一类型。

**语法**

`iif(`*predicate*`,` *ifTrue*`,` *ifFalse*`)`

**参数**

* *predicate*：一个计算结果为 `boolean` 值的表达式。
* ifTrue：如果 predicate 计算结果为 `true`，得以计算的表达式以及其从函数返回的表达式值。
* ifFalse：进行了计算的表达式以及它的从函数返回的表达式值（如果 predicate 计算结果为 `false`）。

**返回**

如果 *predicate* 计算结果为 `true`，此函数返回 *ifTrue* 的值，否则返回 *ifFalse* 的值。

**示例**

```kusto
T 
| extend day = iif(floor(Timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

[`iff()`](ifffunction.md) 的别名。