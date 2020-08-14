---
title: datetime_add() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 datetime_add()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 3361bd7ca3856c79d63ce7191cf5dbe7d46d9e92
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841773"
---
# <a name="datetime_add"></a>datetime_add()

计算新的 [datetime](./scalar-data-types/datetime.md)，方法是将指定 datapart 与指定数量相乘，然后加上指定 [datetime](./scalar-data-types/datetime.md)。

**语法**

`datetime_add(`period`,`amount`,`datetime`)`  

**参数**

* `period`：[字符串](./scalar-data-types/string.md)。 
* `amount`：[整数](./scalar-data-types/int.md)。
* `datetime`：[datetime](./scalar-data-types/datetime.md) 值。

*period* 的可能值： 
- Year
- Quarter
- Month
- Week
- 日期
- Hour
- Minute
- Second
- Millisecond
- Microsecond
- Nanosecond

**返回**

添加特定时间/日期间隔之后的日期。

**示例**

```kusto
print  year = datetime_add('year',1,make_datetime(2017,1,1)),
quarter = datetime_add('quarter',1,make_datetime(2017,1,1)),
month = datetime_add('month',1,make_datetime(2017,1,1)),
week = datetime_add('week',1,make_datetime(2017,1,1)),
day = datetime_add('day',1,make_datetime(2017,1,1)),
hour = datetime_add('hour',1,make_datetime(2017,1,1)),
minute = datetime_add('minute',1,make_datetime(2017,1,1)),
second = datetime_add('second',1,make_datetime(2017,1,1))

```

|year|quarter|月份|week|day|hour|minute|第 2 个|
|---|---|---|---|---|---|---|---|
|2018-01-01 00:00:00.0000000|2017-04-01 00:00:00.0000000|2017-02-01 00:00:00.0000000|2017-01-08 00:00:00.0000000|2017-01-02 00:00:00.0000000|2017-01-01 01:00:00.0000000|2017-01-01 00:01:00.0000000|2017-01-01 00:00:01.0000000|
