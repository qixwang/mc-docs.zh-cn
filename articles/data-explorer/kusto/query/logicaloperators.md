---
title: 逻辑（二元）运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的逻辑（二元）运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 11/14/2018
ms.date: 08/06/2020
ms.openlocfilehash: 1134582704cd82d675b570f3ef2f0896cf38358e
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841305"
---
# <a name="logical-binary-operators"></a>逻辑（二进制）运算符

支持在 `bool` 类型的两个值之间使用以下逻辑运算符：

> [!NOTE]
> 这些逻辑运算符有时称为布尔运算符，有时称为二元运算符。 这些名称都是同义词。

|操作员名称|语法|含义|
|-------------|------|-------|
|相等     |`==`  |如果两个操作数均非 null 且彼此相等，则生成 `true`。 否则为 `false`。|
|不相等   |`!=`  |如果两个操作数不相等或至少有一个为 null，则生成 `true`。 否则为 `true`。|
|逻辑与  |`and` |如果两个操作数都为 `true`，则生成 `true`。|
|逻辑或   |`or`  |如果其中一个操作数是 `true`，则无论其他操作数如何，都将生成 `true`。|

> [!NOTE]
> 考虑到布尔 null 值 `bool(null)` 的行为，我们可以说：两个布尔 null 值既不相等也不不相等（换句话说，`bool(null) == bool(null)` 和 `bool(null) != bool(null)` 都生成值 `false`）。
>
> 另一方面，`and`/`or` 将 null 值视为等效于 `false`，因此 `bool(null) or true` 为 `true`，`bool(null) and true` 为 `false`。