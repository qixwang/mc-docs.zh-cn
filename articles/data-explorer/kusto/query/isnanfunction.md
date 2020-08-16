---
title: isnan() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 isnan()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 58206afbaba6fc72bfd902d4f6c3111afe70bbaa
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841584"
---
# <a name="isnan"></a>isnan()

返回输入是否为非数字 (NAN) 值。  

**语法**

`isnan(`*x*`)`

**参数**

* x：一个实数。

**返回**

如果 x 是 NaN，则为非零值 (true)，否则为零值 (false)。

**另请参阅**

* 若要检查值是否为 null，请参阅 [isnull()](isnullfunction.md)。
* 若要检查值是否为有限值，请参阅 [isfinite()](isfinitefunction.md)。
* 若要检查值是否为无限值，请参阅 [isinf()](isinffunction.md)。

**示例**

```kusto
range x from -1 to 1 step 1
| extend y = (-1*x) 
| extend div = 1.0*x/y
| extend isnan=isnan(div)
```

|x|y|div|isnan|
|---|---|---|---|
|-1|1|-1|0|
|0|0|NaN|1|
|1|-1|-1|0|