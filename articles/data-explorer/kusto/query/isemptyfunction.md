---
title: isempty() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 isempty()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 97b97e31a936e6bc9b26d9dadb688bae46b8bd0c
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841379"
---
# <a name="isempty"></a>isempty()

如果参数为空字符串或为 null，则返回 `true`。
    
```kusto
isempty("") == true
```

**语法**

`isempty(`[*value*]`)`

**返回**

指示参数是空字符串还是 isnull。

|x|isempty(x)
|---|---
| "" | 是
|"x" | false
|parsejson("")|是
|parsejson("[]")|false
|parsejson("{}")|false

**示例**

```kusto
T
| where isempty(fieldName)
| count
```