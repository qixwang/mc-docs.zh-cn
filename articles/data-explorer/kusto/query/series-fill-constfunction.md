---
title: series_fill_const() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 series_fill_const()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 2124d0bf1ea59a88bda714bdd4ef56ed3b4a158c
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841432"
---
# <a name="series_fill_const"></a>series_fill_const()

用指定的常数值替换序列中缺失的值。

采用包含动态数值数组作为输入的表达式，将 missing_value_placeholder 的所有实例替换为指定的 constant_value 并返回生成的数组。

**语法**

`series_fill_const(`*x*`[, `*constant_value*`[,` *missing_value_placeholder*`]])`
* 将返回序列 x，其中的所有 missing_value_placeholder 实例都被替换为 constant_value。

**参数**

* x：动态数组标量表达式（数值数组）。
* constant_value：此参数指定要替换的缺失值的占位符。 默认值为 0。 
* missing_value_placeholder：可选参数，用于指定要替换的缺失值的占位符。 默认值为“`double`(null)”。

**备注**
* 可以通过使用 `default = ` DefaultValue 语法（或直接省略此项，这样就会假定其为 0）来创建使用常量值填充的序列。 有关详细信息，请参阅 [make-series](make-seriesoperator.md)。

```kusto
make-series num=count() default=-1 on TimeStamp from ago(1d) to ago(1h) step 1h by Os, Browser
```
  
* 若要在 [make-series](make-seriesoperator.md) 之后应用任何内插函数，请指定“null”作为默认值： 

```kusto
make-series num=count() default=long(null) on TimeStamp from ago(1d) to ago(1h) step 1h by Os, Browser
```
  
* missing_value_placeholder 可以是将转换为实际元素类型的任何类型。 因此，“`double`(null)”、“`long`(null)”或“`int`(null)”的含义相同。
* 此函数会保留数组元素的原始类型。 

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
let data = datatable(`arr`: dynamic)
[
    dynamic([111,null,36,41,23,null,16,61,33,null,null])   
];
data 
| project arr, 
          fill_const1 = series_fill_const(arr, 0.0),
          fill_const2 = series_fill_const(arr, -1)  
```

|`arr`|`fill_const1`|`fill_const2`|
|---|---|---|
|[111,null,36,41,23,null,16,61,33,null,null]|[111,0.0,36,41,23,0.0,16,61,33,0.0,0.0]|[111,-1,36,41,23,-1,16,61,33,-1,-1]|
