---
title: isutf8() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 isutf8()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: e06eba6e5135d7b091eb7b11df37b3dfb7d9e8e5
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841627"
---
# <a name="isutf8"></a>isutf8()

如果参数是有效的 utf8 字符串，则返回 `true`。
    
```kusto
isutf8("some string") == true
```

**语法**

`isutf8(`[*value*]`)`

**返回**

指示参数是否为有效的 utf8 字符串。

**示例**

```kusto
T
| where isutf8(fieldName)
| count
```