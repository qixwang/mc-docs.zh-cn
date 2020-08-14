---
title: dayofweek() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 dayofweek()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 10440b688abd520b7c734c85b61515e8b64063df
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841592"
---
# <a name="dayofweek"></a>dayofweek()

返回自上个星期日以来的整数天数，用作 `timespan`。

```kusto
dayofweek(datetime(2015-12-14)) == 1d  // Monday
```

**语法**

`dayofweek(`*a_date*`)`

**参数**

* `a_date`：`datetime`。

**返回**

`timespan`从上个星期日的午夜开始，向下舍入为整数天数。

**示例**

```kusto
dayofweek(datetime(1947-11-30 10:00:05))  // time(0.00:00:00), indicating Sunday
dayofweek(datetime(1970-05-11))           // time(1.00:00:00), indicating Monday
```
