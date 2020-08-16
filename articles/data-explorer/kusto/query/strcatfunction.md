---
title: strcat() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 strcat()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: b57348f6c15075df4c35e2b993390a4e76c36c34
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841704"
---
# <a name="strcat"></a>strcat()

连接 1 到 64 个参数。

* 如果参数不是字符串类型，则会将它们强制转换为字符串。

**语法**

`strcat(`*argument1*, *argument2*[, *argumentN*]`)`

**参数**

* argument1 ... argumentN：要连接的表达式。

**返回**

参数（已连接到一个字符串）。

**示例**
  
   ```kusto
print str = strcat("hello", " ", "world")
```

|str|
|---|
|hello world|
