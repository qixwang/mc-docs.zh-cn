---
title: radians() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 radians()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 8aedeb1dcb85bae856422913defc2f9f66971959
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841514"
---
# <a name="radians"></a>radians()

使用公式 `radians = (PI / 180 ) * angle_in_degrees`，将以度表示的角度值转换为以弧度表示的值

**语法**

`radians(`*a*`)`

**参数**

* a：以度表示的角度（实数）。

**返回**

* 指定角度（度）的相应角度（弧度）。 

**示例**

```kusto
print radians0 = radians(90), radians1 = radians(180), radians2 = radians(360) 

```

|radians0|radians1|radians2|
|---|---|---|
|1.5707963267949|3.14159265358979|6.28318530717959|