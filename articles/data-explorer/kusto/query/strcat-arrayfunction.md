---
title: strcat_array() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 strcat_array()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: ea0e9567629d39d679f2b4578b27b44fe5ecd2ca
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841556"
---
# <a name="strcat_array"></a>strcat_array()

使用指定的分隔符创建一个包含数组值的连接字符串。
    
**语法**

`strcat_array(`*array*, *delimiter*`)`

**参数**

* *array*：一个 `dynamic` 值，表示要连接的值的数组。
* delimeter：一个 `string` 值，将用于连接 array 中的值

**返回**

数组值，连接到一个字符串。

**示例**
  
```kusto
print str = strcat_array(dynamic([1, 2, 3]), "->")
```

|str|
|---|
|1->2->3|