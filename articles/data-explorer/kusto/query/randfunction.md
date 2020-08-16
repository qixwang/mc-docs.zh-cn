---
title: rand() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 rand()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 9ff7e084a017831a4f6f8da920f5376a0748869e
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841265"
---
# <a name="rand"></a>rand()

返回随机数。

```kusto
rand()
rand(1000)
```

**语法**

* `rand()` - 返回一个 `real` 类型的值，该值在 [0.0, 1.0) 范围内均匀分布。
* `rand(` N `)` - 返回从集 {0.0, 1.0, ..., N - 1} 中选择的 `real` 类型的均匀分布的值。