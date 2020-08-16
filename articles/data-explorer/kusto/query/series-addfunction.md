---
title: series_add() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 series_add()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/06/2020
ms.openlocfilehash: 6dff579c928f13fde40dad30f4216c4746dd648f
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841168"
---
# <a name="series_add"></a>series_add()

计算两个数值序列输入的元素对应加法。

**语法**

`series_add(`*series1*`,` *series2*`)`

**参数**

* series1、series2：输入数值数组，按对应元素相加获得动态数组结果。 所有参数都必须是动态数组。 

**返回**

在两个输入之间进行按元素加法运算计算出的动态数组。 任何非数值元素或非现有元素（不同大小的数组）都会生成 `null` 元素值。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 3 step 1
| extend y = x * 2
| extend z = y * 2
| project s1 = pack_array(x,y,z), s2 = pack_array(z, y, x)
| extend s1_add_s2 = series_add(s1, s2)
```

|s1|s2|s1_add_s2|
|---|---|---|
|[1,2,4]|[4,2,1]|[5,4,5]|
|[2,4,8]|[8,4,2]|[10,8,10]|
|[3,6,12]|[12,6,3]|[15,12,15]|
