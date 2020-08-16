---
title: week_of_year() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 week_of_year()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/18/2020
ms.date: 08/06/2020
ms.openlocfilehash: df16b9fc6e3f3f2a5e7cc0d224bb44c3366e43cb
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841325"
---
# <a name="week_of_year"></a>week_of_year()

返回一个表示周数的整数。 根据 ISO 8601，周数从一年的第一周算起，该周包括第一个周四。

```kusto
week_of_year(datetime("2015-12-14"))
```

**语法**

`week_of_year(`*a_date*`)`

**参数**

* `a_date`：`datetime`。

**返回**

`week number` - 包含给定日期的周数。

**示例**

|输入                                    |输出|
|-----------------------------------------|------|
|`week_of_year(datetime(2020-12-31))`     |`53`  |
|`week_of_year(datetime(2020-06-15))`     |`25`  |
|`week_of_year(datetime(1970-01-01))`     |`1`   |
|`week_of_year(datetime(2000-01-01))`     |`52`  |

> [!NOTE]
> `weekofyear()` 是此函数的过时变体。 `weekofyear()` 不符合 ISO 8601；一年的第一周被定义为一年中包含第一个周三的那一周。
函数 `week_of_year()` 的当前版本符合 ISO 8601；一年的第一周被定义为一年中包含第一个周四的那一周。