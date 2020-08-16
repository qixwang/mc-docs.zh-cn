---
title: series_stats() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 series_stats()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/10/2020
ms.date: 08/06/2020
ms.openlocfilehash: f1ceb0d01173b98d98efc4d45de1e42b840ea957
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841225"
---
# <a name="series_stats"></a>series_stats()

`series_stats()` 返回多个列中的序列的统计信息。  

`series_stats()` 函数接受包含动态数值数组的列作为输入，并计算以下各列：
* `min`：输入数组中的最小值
* `min_idx`：输入数组中的最小值的第一个位置
* `max`：输入数组中的最大值
* `max_idx`：输入数组中的最大值的第一个位置
* `avg`：输入数组的平均值
* `variance`：输入数组的样本方差
* `stdev`：输入数组的样本标准偏差

> [!NOTE] 
> 此函数将返回多个列，因此不能将其用作另一个函数的参数。

**语法**

project `series_stats(`*x* `[,`*ignore_nonfinite*`])` 或 extend `series_stats(`*x*`)` 返回前面提到的所有列，名称如下：series_stats_x_min、series_stats_x_min_idx 等。
 
project (m, mi)=`series_stats(`*x*`)` 或 extend (m, mi)=`series_stats(`*x*`)` 返回以下列：m (min) 和 mi (min_idx)。

**参数**

* x：动态数组单元格（数值数组）。 
* ignore_nonfinite：布尔值（可选，默认值：`false`）标志，该标志指定是否在计算统计信息的同时忽略非有限值（null、NAN、inf 等）。 如果设置为 `false`，则返回的值为 `null`（如果数组中存在非有限值）。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print x=dynamic([23,46,23,87,4,8,3,75,2,56,13,75,32,16,29]) 
| project series_stats(x)

```

|series_stats_x_min|series_stats_x_min_idx|series_stats_x_max|series_stats_x_max_idx|series_stats_x_avg|series_stats_x_stdev|series_stats_x_variance|
|---|---|---|---|---|---|---|
|2|8|87|3|32.8|28.5036338535483|812.457142857143|
