---
title: make_timespan() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 make_timespan()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 544e6d52028886f921753021e503ef0340fdab86
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841369"
---
# <a name="make_timespan"></a>make_timespan()

根据指定的时间段创建一个 [timespan](./scalar-data-types/timespan.md) 标量值。

```kusto
make_timespan(1,12,30,55.123) == time(1.12:30:55.123)
```

**语法**

`make_timespan(`*hour*,*minute*`)`

`make_timespan(`*hour*,*minute*,*second*`)`

`make_timespan(`*day*,*hour*,*minute*,*second*`)`

**参数**

* day：天（必须是正整数值）
* hour：小时（必须是 0 到 23 的正整数值）
* minute：分钟（必须是 0 到 59 的正整数值）
* second：秒（必须是 0 到 59.9999999 的实数值）

**返回**

若创建成功，结果将是 [timespan](./scalar-data-types/timespan.md) 值，否则，结果将为 null。
 
**示例**

```kusto
print ['timespan'] = make_timespan(1,12,30,55.123)

```

|timespan|
|---|
|1.12:30:55.1230000|


