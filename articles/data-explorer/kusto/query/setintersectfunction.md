---
title: set_intersect() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 set_intersect()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 06/02/2019
ms.date: 08/06/2020
ms.openlocfilehash: 7e0ac3ec12299a3e27517d648ca9f2fd0328710e
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841568"
---
# <a name="set_intersect"></a>set_intersect()

返回一个 `dynamic` 数组，其中包含在所有数组 (arr1 ∩ arr2 ∩ ...) 中的所有非重复值的集合。

**语法**

`set_intersect(`*arr1*`, `*arr2*`[`,` *arr3*, ...])`

**参数**

* arr1...arrN：用于创建交集的输入数组（至少两个数组）。 所有参数都必须是动态数组。 有关详细信息，请参阅 [pack_array](packarrayfunction.md)。 

**返回**

返回一个动态数组，其中包含在所有数组中的所有非重复值的集合。 请参阅 [`set_union()`](setunionfunction.md) 和 [`set_difference()`](setdifferencefunction.md)。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 3 step 1
| extend y = x * 2
| extend z = y * 2
| extend w = z * 2
| extend a1 = pack_array(x,y,x,z), a2 = pack_array(x, y), a3 = pack_array(w,x)
| project set_intersect(a1, a2, a3)
```

|Column1|
|---|
| [1]|
|[2]|
|[3]|

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print arr = set_intersect(dynamic([1, 2, 3]), dynamic([4,5]))
```

|arr|
|---|
|[]|
