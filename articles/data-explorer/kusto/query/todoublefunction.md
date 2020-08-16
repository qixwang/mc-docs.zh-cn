---
title: todouble()/toreal() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 todouble()/toreal()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 21116770f1dc0836bd7c8e457daadf2e8c18d450
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841702"
---
# <a name="todouble-toreal"></a>todouble(), toreal()

将输入转换为 `real` 类型的值。 （`todouble()` 和 `toreal()` 是同义函数。）

```kusto
toreal("123.4") == 123.4
```

**语法**

`toreal(`*Expr*`)`
`todouble(`*Expr*`)`

**参数**

* Expr：一个表达式，其值将转换为 `real` 类型的值。

**返回**

如果转换成功，则结果为 `real` 类型的值。
如果转换不成功，则结果为 `real(null)` 值。

*注意*：如果可能，最好使用 [double() 或 real()](./scalar-data-types/real.md)。