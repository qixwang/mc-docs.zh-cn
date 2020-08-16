---
title: startofweek() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 startofweek()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: c3dbf0bd769f0cd4f21bb7a75c303ffba857bd86
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841557"
---
# <a name="startofweek"></a>startofweek()

返回包含日期的一周的起点，根据偏移量移动（如提供）。

一周的起点被视为周日。

**语法**

`startofweek(`*date* [`,`*offset*]`)`

**参数**

* `date`：输入日期。
* `offset`：输入日期中的可选偏移周数（整数，默认值为 0）。

**返回**

一个日期/时间，表示给定 date 值的那一周的开始（如果指定了偏移量，则还包含该信息）。

**示例**

```kusto
  range offset from -1 to 1 step 1
 | project weekStart = startofweek(datetime(2017-01-01 10:10:17), offset) 
```

|weekStart|
|---|
|2016-12-25 00:00:00.0000000|
|2017-01-01 00:00:00.0000000|
|2017-01-08 00:00:00.0000000|