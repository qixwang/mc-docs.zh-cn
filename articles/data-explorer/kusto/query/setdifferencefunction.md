---
title: set_difference() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 set_difference()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 06/02/2019
ms.date: 08/06/2020
ms.openlocfilehash: e71aef21e134c2de7a6fe55dcfcf3db5e8311225
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841172"
---
# <a name="set_difference"></a>set_difference()

返回一个 `dynamic` (JSON) 数组，其中包含存在于第一个数组中但不存在于其他数组中的所有非重复值的集合 - (((arr1 \ arr2) \ arr3) \ ...)。

**语法**

`set_difference(`*arr1*`, `*arr2*`[`,` *arr3*, ...])`

**参数**

* arr1...arrN：用于创建差异集的输入数组（至少两个数组）。 所有参数都必须是动态数组（请参阅 [pack_array](packarrayfunction.md)）。 

**返回**

返回一个动态数组，其中包含存在于 arr1 中但不存在于其他数组中的所有非重复值的集合。 请参阅 [`set_union()`](setunionfunction.md) 和 [`set_intersect()`](setintersectfunction.md)。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 3 step 1
| extend y = x * 2
| extend z = y * 2
| extend w = z * 2
| extend a1 = pack_array(x,y,x,z), a2 = pack_array(x, y), a3 = pack_array(x,y,w)
| project set_difference(a1, a2, a3)
```

|Column1|
|---|
|[4]|
|[8]|
| [12]|

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print arr = set_difference(dynamic([1,2,3]), dynamic([1,2,3]))
```

|arr|
|---|
|[]|