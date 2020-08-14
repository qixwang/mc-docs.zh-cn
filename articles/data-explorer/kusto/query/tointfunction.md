---
title: toint() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 toint()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 498e233ba0bdf4c848801af3153eb20895fc1237
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841693"
---
# <a name="toint"></a>toint()

将输入转换为 integer（带符号的 32 位）数字表示形式。

```kusto
toint("123") == 123s
```

**语法**

`toint(`Expr`)`

**参数**

* Expr：将转换为整数的表达式。 

**返回**

如果转换成功，结果将为整数。
如果转换未成功，结果将为 `null`。
 
*注意*：如果可能，建议使用 [int()](./scalar-data-types/int.md)。