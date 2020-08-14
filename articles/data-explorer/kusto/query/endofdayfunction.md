---
title: endofday() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 endofday()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 5889f8258aaac2c03c6d32b0648c9f70b0f96ebc
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841285"
---
# <a name="endofday"></a>endofday()

返回包含日期的一天结束的时间，根据偏移量移动（如提供）。

**语法**

`endofday(`*date* [`,`*offset*]`)`

**参数**

* `date`：输入日期。
* `offset`：输入日期（整数，默认值为 0）中的可选偏移天数。

**返回**

表示给定 date 值一天结束的日期/时间（如果指定了偏移量，还包含该信息）。

**示例**

```kusto
  range offset from -1 to 1 step 1
 | project dayEnd = endofday(datetime(2017-01-01 10:10:17), offset) 
```

|dayEnd|
|---|
|2016-12-31 23:59:59.9999999|
|2017-01-01 23:59:59.9999999|
|2017-01-02 23:59:59.9999999|