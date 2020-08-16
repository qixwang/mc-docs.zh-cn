---
title: endofweek() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 endofweek()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 0627d54bb5027229c9b8839735fc36d47b03efda
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841275"
---
# <a name="endofweek"></a>endofweek()

返回包含日期的一周的终点，根据偏移量移动（如提供）。

一周的最后一天被视为周六。

**语法**

`endofweek(`*date* [`,`*offset*]`)`

**参数**

* `date`：输入日期。
* `offset`：输入日期中的可选偏移周数（整数，默认值为 0）。

**返回**

一个日期/时间，表示给定 date 值的那一周的结束（如果指定了偏移量，则还包含该信息）。

**示例**

```kusto
  range offset from -1 to 1 step 1
 | project weekEnd = endofweek(datetime(2017-01-01 10:10:17), offset)  

```

|weekEnd|
|---|
|2016-12-31 23:59:59.9999999|
|2017-01-07 23:59:59.9999999|
|2017-01-14 23:59:59.9999999|