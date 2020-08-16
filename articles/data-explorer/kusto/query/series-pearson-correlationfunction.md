---
title: series_pearson_correlation() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 series_pearson_correlation()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/31/2019
ms.date: 08/06/2020
ms.openlocfilehash: 2249047dbd0ee3d3800ab27689616dcbafffe71c
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841229"
---
# <a name="series_pearson_correlation"></a>series_pearson_correlation()

计算两个数值序列输入的皮尔逊相关系数。

**语法**

`series_pearson_correlation(`*Series1*`,` *Series2*`)`

**参数**

* Series1、Series2：用于计算相关系数的输入数值数组。 所有参数都必须是长度相同的动态数组。 

**返回**

两个输入之间计算的皮尔逊相关系数。 任何非数值元素或非现有元素（不同大小的数组）都会生成 `null` 结果。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range s1 from 1 to 5 step 1 | extend s2 = 2*s1 // Perfect correlation
| summarize s1 = make_list(s1), s2 = make_list(s2)
| extend correlation_coefficient = series_pearson_correlation(s1,s2)
```

|s1|s2|correlation_coefficient|
|---|---|---|
|[1,2,3,4,5]|[2,4,6,8,10]|1|
