---
title: series_multiply() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 series_multiply()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/06/2020
ms.openlocfilehash: 44790921d8907eda23ac9606156b4057ccc2fece
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841233"
---
# <a name="series_multiply"></a>series_multiply()

计算两个数值序列输入的元素对应乘法。

**语法**

`series_multiply(`series1`,` series2`)` 

**参数**

* series1, series2：输入数值数组，按对应元素相乘获得动态数组结果。 所有参数都必须是动态数组。 

**返回**

两个输入之间的元素对应乘法运算的计算结果的动态数组。 任何非数值元素或非现有元素（不同大小的数组）都会生成 `null` 元素值。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 3 step 1
| extend y = x * 2
| extend z = y * 2
| project s1 = pack_array(x,y,z), s2 = pack_array(z, y, x)
| extend s1_multiply_s2 = series_multiply(s1, s2)
```

|s1         |s2|        s1_multiply_s2|
|---|---|---|
|[1,2,4]    |[4,2,1]|   [4,4,4]|
|[2,4,8]    |[8,4,2]|   [16,16,16]|
|[3,6,12]   |[12,6,3]|  [36,36,36]|
