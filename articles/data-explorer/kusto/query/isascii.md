---
title: isascii() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 isascii()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 6ff2b68e2b84ea445c8769282f9fc48d9d8b45cf
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841676"
---
# <a name="isascii"></a>isascii()

如果参数是有效的 ascii 字符串，则返回 `true`。
    
```kusto
isascii("some string") == true
```

**语法**

`isascii(`[*value*]`)`

**返回**

指示参数是否为有效的 ascii 字符串。

**示例**

```kusto
T
| where isascii(fieldName)
| count
```