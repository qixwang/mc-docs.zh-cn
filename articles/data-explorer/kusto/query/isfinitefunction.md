---
title: isfinite() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 isfinite()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: ea1f9143ff5b543e5212511fd9cbc9c8fe4955a9
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841585"
---
# <a name="isfinite"></a>isfinite()

返回输入是否为有限值（既不是无穷值也不是 NAN）。

**语法**

`isfinite(`*x*`)`

**参数**

* x：一个实数。

**返回**

如果 x 是有限值，则为非零值 (true)，否则为零值 (false)。

**另请参阅**

* 若要检查值是否为 null，请参阅 [isnull()](isnullfunction.md)。
* 若要检查值是否为无限值，请参阅 [isinf()](isinffunction.md)。
* 若要检查值是否为 NAN（非数字），请参阅 [isnan()](isnanfunction.md)。

**示例**

```kusto
range x from -1 to 1 step 1
| extend y = 0.0
| extend div = 1.0*x/y
| extend isfinite=isfinite(div)
```

|x|y|div|isfinite|
|---|---|---|---|
|-1|0|-∞|0|
|0|0|NaN|0|
|1|0|∞|0|