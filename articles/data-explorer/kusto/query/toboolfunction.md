---
title: tobool() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 tobool()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 06f9ee3be55bd69bb5abc7870f482e29c7fb30b4
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841212"
---
# <a name="tobool"></a>tobool()

将输入转换为布尔值（带符号的 8 位）表示形式。

```kusto
tobool("true") == true
tobool("false") == false
tobool(1) == true
tobool(123) == true
```

**语法**

`tobool(`*Expr*`)`
`toboolean(`*Expr*`)` (alias)

**参数**

* Expr：将转换为布尔值的表达式。 

**返回**

如果转换成功，则结果将为布尔值。
如果转换不成功，结果将为 `null`。
