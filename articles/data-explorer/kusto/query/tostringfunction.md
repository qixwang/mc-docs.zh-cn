---
title: tostring() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 tostring()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 206a3c3cca18a094536ec8aa0b85c2870ba182d9
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841416"
---
# <a name="tostring"></a>tostring()

将输入转换为字符串表示形式。

```kusto
tostring(123) == "123"
```

**语法**

`tostring(`*`Expr`*`)`

**参数**

* *`Expr`* ：将转换为字符串的表达式。 

**返回**

如果 `Expr` 值不为 null，则结果将为 `Expr` 的字符串表示形式。
如果 *`Expr`* 值为 null，则结果将为空字符串。
 