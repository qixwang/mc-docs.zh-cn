---
title: strcat_delim() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 strcat_delim()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: f4a79e24beb43fa399e3e6a6d93ffbb4b9c4cd98
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841552"
---
# <a name="strcat_delim"></a>strcat_delim()

连接 2 到 64 个参数（分隔符作为第一个参数提供）。

 * 如果参数不是字符串类型，则会将它们强制转换为字符串。

**语法**

`strcat_delim(`*delimiter*, *argument1*, *argument2*[ , *argumentN*]`)`

**参数**

* delimiter：要用作分隔符的字符串表达式。
* argument1 ... argumentN：要连接的表达式。

**返回**

参数（已使用 delimiter 连接到单个字符串）。

**示例**

```kusto
print st = strcat_delim('-', 1, '2', 'A', 1s)

```

|st|
|---|
|1-2-A-00:00:01|
