---
title: tolong() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 tolong()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 4fbc0165d561376843f946df3cc0a318b9599bd4
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841211"
---
# <a name="tolong"></a>tolong()

将输入转换为 long（带符号的 64 位）数字表示形式。

```kusto
tolong("123") == 123
```

**语法**

`tolong(`*Expr*`)`

**参数**

* Expr：将转换为 long 的表达式。 

**返回**

如果转换成功，则结果将是一个 long 类型的数字。
如果转换未成功，结果将是 `null`。
 
*注意*：尽可能首选使用 [long()](./scalar-data-types/long.md)。