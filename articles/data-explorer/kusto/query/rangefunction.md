---
title: range() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 range()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 2ed20fe80153c0a4e336a0bce4566da8712fa364
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841512"
---
# <a name="range"></a>range()

生成包含一系列等间距值的动态数组。

**语法**

`range(`start`,` stop[`,` step]`)`   

**参数**

* *start*：生成数组中第一个元素的值。 
* stop：生成数组中最后一个元素的值，或大于生成数组中的最后一个元素且在 start 中 step 的整数倍以内的最小值。
* step：数组中两个连续元素之间的差异。 step 的默认值为 `1`（数字）和 `1h`（`timespan` 或 `datetime`）

**示例**

以下示例返回 `[1, 4, 7]`：

```kusto
T | extend r = range(1, 8, 3)
```

以下示例返回包含 2015 年所有天数的数组：

```kusto
T | extend r = range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

以下示例返回 `[1,2,3]`：

```kusto
range(1, 3)
```

以下示例返回 `["01:00:00","02:00:00","03:00:00","04:00:00","05:00:00"]`：

```kusto
range(1h, 5h)
```
