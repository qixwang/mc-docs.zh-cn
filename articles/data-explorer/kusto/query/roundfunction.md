---
title: round() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 round()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: f8122d869a993c4553bfa74c603ef38e6b2b25f5
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841494"
---
# <a name="round"></a>round()

返回舍入到指定精度的源。

**语法**

`round(`*source* [`,` *Precision*]`)`

**参数**

* *source*：计算舍入时基于的源标量。
* *精度*：源将舍入到的位数（默认值为 0）。

**返回**

舍入到指定精度的源。

Round 与 [`bin()`](binfunction.md)/[`floor()`](floorfunction.md) 的不同之处在于，前者将数字舍入到特定的位数，后者将值舍入到给定 bin 大小的整数倍（round(2.15, 1) 返回 2.2，而 bin(2.15, 1) 则返回 2）。
 

**示例**

```kusto
round(2.15, 1)                   // 2.2
round(2.15) (which is the same as round(2.15, 0))                   // 2
round(-50.55, -2)                   // -100
round(21.5, -1)                   // 20
```