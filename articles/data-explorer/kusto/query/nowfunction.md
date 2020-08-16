---
title: now() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 now()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: ffadc6735bd2de925a60a809aea66a2f3da247c1
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841520"
---
# <a name="now"></a>now()

返回当前的 UTC 时钟时间，偏移量取决于给定的时间跨度（可选）。
此函数可在语句中多次使用，并且所有实例引用的时钟时间均相同。

```kusto
now()
now(-2d)
```

**语法**

`now(`[*offset*]`)`

**参数**

* *offset*：`timespan`，已添加到当前的 UTC 时钟时间。 默认值：0。

**返回**

当前 UTC 时钟时间，用作 `datetime`。

`now()` + *offset* 

**示例**

确定 predicate 标识事件后的时间间隔：

```kusto
T | where ... | extend Elapsed=now() - Timestamp
```