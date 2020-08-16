---
title: degrees() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 degrees()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: c4ff8fdd7da09cb14a14aa92d086931facdba86f
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841288"
---
# <a name="degrees"></a>degrees()

使用公式 `degrees = (180 / PI ) * angle_in_radians`，将以弧度表示的角度值转换为以度表示的值

**语法**

`degrees(`*a*`)`

**参数**

* a：以弧度表示的角度（实数）。

**返回**

* 指定角度（弧度）的相应角度（度）。 

**示例**

```kusto
print degrees0 = degrees(pi()/4), degrees1 = degrees(pi()*1.5), degrees2 = degrees(0)

```

|degrees0|degrees1|degrees2|
|---|---|---|
|45|270|0|
