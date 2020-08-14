---
title: endofyear() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 endofyear()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: fc57fb082b45f4121aa1df3552c99e069bbc44ed
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841274"
---
# <a name="endofyear"></a>endofyear()

返回包含日期的一年的终点，根据偏移量移动（如提供）。

**语法**

`endofyear(`*date* [`,`*offset*]`)`

**参数**

* `date`：输入日期。
* `offset`：输入日期（整数，默认值为 0）中的可选偏移年数。

**返回**

表示给定“日期”值的年份结束的日期/时间（带偏移量，如果指定了的话）。

**示例**

```kusto
  range offset from -1 to 1 step 1
 | project yearEnd = endofyear(datetime(2017-01-01 10:10:17), offset) 
```

|yearEnd|
|---|
|2016-12-31 23:59:59.9999999|
|2017-12-31 23:59:59.9999999|
|2018-12-31 23:59:59.9999999|