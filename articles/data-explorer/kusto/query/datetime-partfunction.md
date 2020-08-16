---
title: datetime_part() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 datetime_part()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/18/2020
ms.date: 08/06/2020
ms.openlocfilehash: 264d20812fcfdee8c3a2fe9d26c2c76bf714da31
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841600"
---
# <a name="datetime_part"></a>datetime_part()

提取所请求的日期部分作为整数值。

```kusto
datetime_part("Day",datetime(2015-12-14))
```

**语法**

`datetime_part(`*part*`,`*datetime*`)`

**参数**

* `date`: `datetime`
* `part`: `string`

`part` 的可能值： 
- Year
- Quarter
- Month
- week_of_year
- 日期
- DayOfYear
- Hour
- Minute
- Second
- Millisecond
- Microsecond
- Nanosecond

**返回**

一个表示已提取部分的整数。

**注意**

`week_of_year` 返回一个表示周数的整数。 周数从一年的第一周算起，该周包括第一个周四。

**示例**

```kusto
let dt = datetime(2017-10-30 01:02:03.7654321); 
print 
year = datetime_part("year", dt),
quarter = datetime_part("quarter", dt),
month = datetime_part("month", dt),
weekOfYear = datetime_part("week_of_year", dt),
day = datetime_part("day", dt),
dayOfYear = datetime_part("dayOfYear", dt),
hour = datetime_part("hour", dt),
minute = datetime_part("minute", dt),
second = datetime_part("second", dt),
millisecond = datetime_part("millisecond", dt),
microsecond = datetime_part("microsecond", dt),
nanosecond = datetime_part("nanosecond", dt)

```

|year|quarter|月份|weekOfYear|day|dayOfYear|hour|minute|第 2 个|毫秒|微秒|纳秒|
|---|---|---|---|---|---|---|---|---|---|---|---|
|2017|4|10|44|30|303|1|2|3|765|765432|765432100|

> [!NOTE]
> `weekofyear` 是 `week_of_year` 部分的已过时变体。 `weekofyear` 不符合 ISO 8601；一年的第一周被定义为一年中包含第一个周三的那一周。
`week_of_year` 符合 ISO 8601；一年的第一周被定义为一年中包含第一个周四的那一周。