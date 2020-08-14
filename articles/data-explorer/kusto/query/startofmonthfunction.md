---
title: Startofmonth() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 startofmonth()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 2da2b55d8ba9cccf4cc1472805fd1057dbebbf77
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841715"
---
# <a name="startofmonth"></a>startofmonth()

返回包含日期的一月的起点，根据偏移量移动（如提供）。

**语法**

`startofmonth(`*date* [`,`*offset*]`)`

**参数**

* `date`：输入日期。
* `offset`：输入日期中的可选偏移月数（整数，默认值为 0）。

**返回**

表示给定“日期”值的月份开始的日期/时间（带偏移量，如果指定了的话）。

**示例**

```kusto
  range offset from -1 to 1 step 1
 | project monthStart = startofmonth(datetime(2017-01-01 10:10:17), offset) 
```

|monthStart|
|---|
|2016-12-01 00:00:00.0000000|
|2017-01-01 00:00:00.0000000|
|2017-02-01 00:00:00.0000000|