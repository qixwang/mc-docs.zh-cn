---
title: not() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 not()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 19c09858389bcb3e5face701dc8d2d86cd1fd30f
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841733"
---
# <a name="not"></a>not()

对其 `bool` 参数的值取反。

```kusto
not(false) == true
```

**语法**

`not(`expr`)`

**参数**

* expr：要取反的 `bool` 表达式。

**返回**

返回其 `bool` 参数的反向逻辑值。