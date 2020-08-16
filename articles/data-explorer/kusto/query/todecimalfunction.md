---
title: todecimal() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 todecimal()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 6a4cb6f8fe6ed4b91286c455471c45f1caca89e9
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841703"
---
# <a name="todecimal"></a>todecimal()

将输入转换为十进制数表示形式。

```kusto
todecimal("123.45678") == decimal(123.45678)
```

**语法**

`todecimal(`*Expr*`)`

**参数**

* Expr：将转换为十进制的表达式。 

**返回**

如果转换成功，则结果将为十进制数。
如果转换未成功，结果将是 `null`。
 
*注意*：尽可能首选使用 [real()](./scalar-data-types/real.md)。