---
title: series_fill_backward() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 series_fill_backward()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: f9d34ce13d4615c154828946d5952daf60a76b68
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841434"
---
# <a name="series_fill_backward"></a>series_fill_backward()

在序列中对缺失值执行后向填充内插。

包含动态数值数组的表达式为输入。 该函数将 missing_value_placeholder 的所有实例替换为离其右侧最近的值（missing_value_placeholder 除外），并返回生成的数组。 保留 missing_value_placeholder 的最右侧实例。

**语法**

`series_fill_backward(`*x*`[, `*missing_value_placeholder*`])`
* 将返回序列 x，其中前向填充了 missing_value_placeholder 的所有实例。

**参数**

* x：动态数组标量表达式（数值数组）。
* missing_value_placeholder：此可选参数为缺失值指定占位符。 默认值为“`double`(null)”。

**备注**

* 指定“null”作为默认值，以便在 [make-series](make-seriesoperator.md) 之后应用任何内插函数： 

```kusto
make-series num=count() default=long(null) on TimeStamp from ago(1d) to ago(1h) step 1h by Os, Browser
```

* missing_value_placeholder 可以是将要转换为实际元素类型的任何类型。 `double`(null)、`long`(null) 和 `int`(null) 具有相同的含义。
* 如果 missing_value_placeholder 为 `double`(null) 或被省略（二者含义相同），则结果可能包含 null 值。 若要填充这些 null 值，请使用其他内插函数。 目前只有 [series_outliers()](series-outliersfunction.md) 支持在输入数组中使用 null 值。
* 此函数会保留数组元素的原始类型。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
let data = datatable(arr: dynamic)
[
    dynamic([111,null,36,41,null,null,16,61,33,null,null])   
];
data 
| project arr, 
          fill_forward = series_fill_backward(arr)

```

|`arr`|`fill_forward`|
|---|---|
|[111,null,36,41,null,null,16,61,33,null,null]|[111,36,36,41,16, 16,16,61,33,null,null]|

  
使用 [series_fill_forward](series-fill-forwardfunction.md) 或 [series-fill-const](series-fill-constfunction.md) 来完成以上数组的内插。
