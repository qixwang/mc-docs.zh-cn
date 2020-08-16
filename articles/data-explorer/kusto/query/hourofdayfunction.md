---
title: hourofday() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 hourofday()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: c6f493a45dae47d4c4bf043a5615c84bd18c51a3
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841395"
---
# <a name="hourofday"></a>hourofday()

返回一个整数，该整数表示给定日期的第几个小时

```kusto
hourofday(datetime(2015-12-14 18:54)) == 18
```

**语法**

`hourofday(`*a_date*`)`

**参数**

* `a_date`：`datetime`。

**返回**

一天的 `hour number` (0-23)。