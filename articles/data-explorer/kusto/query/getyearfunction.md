---
title: getyear() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 getyear()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 88a74b7f8b2a1aaccc7a7b29623c25695b3bc614
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841742"
---
# <a name="getyear"></a>getyear()

返回 `datetime` 参数的年份部分。

**示例**

```kusto
T
| extend year = getyear(datetime(2015-10-12))
// year == 2015
```