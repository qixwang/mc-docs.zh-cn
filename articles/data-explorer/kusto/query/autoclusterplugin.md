---
title: autocluster 插件 - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的 autocluster 插件。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 3a539b27f7d88ffe15f0ccd5fb4150bbe71d2148
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509161"
---
# <a name="autocluster-plugin"></a>autocluster 插件

```kusto
T | evaluate autocluster()
```

`autocluster` 在数据中查找离散属性（维度）的常见模式。 然后，它将原始查询的结果（无论是 100 行还是 10 万行）减少到少量模式。 开发该插件的目的是为了帮助分析故障（如异常或崩溃），但该插件也有可能用于处理任何筛选出的数据集。

## <a name="syntax"></a>语法

`T | evaluate autocluster(` *arguments* `)`

## <a name="returns"></a>返回

`autocluster` 插件返回一组（通常为一小组）模式。 这些模式在多个离散属性间捕获具有共享常见值的数据部分。 结果中的每种模式均由一行表示。

第一列是段 ID。 后两列是此模式从原始查询外所捕获行的数量和百分比。 其余列均来自原始查询。 它们的值是来自列中的特定值，或者是表示变量值的通配符值（默认为 null）。

这些模式并不是截然不同的，有可能会重叠，且通常不包括所有原始行。 某些行可能不属于任何模式。

> [!TIP]
> 在输入管道中使用 [where](./whereoperator.md) 和 [project](./projectoperator.md) 可将数据缩减到仅剩所需数据。
>
> 找到所需行时，可通过将该行的特定值添加到 `where` 筛选器，来对其进行深入研究。

**参数（全部可选）**

`T | evaluate autocluster(`[*SizeWeight*, *WeightColumn*, *NumSeeds*, *CustomWildcard*, *CustomWildcard*, ...]`)`

所有参数都为可选参数，但必须按上述方式进行排序。 若要指示应当使用默认值，请输入字符串波形值“~”（请参阅表中的“示例”列）。

|参数        | 类型、范围、默认值              |说明                | 示例                                        |
|----------------|-----------------------------------|---------------------------|------------------------------------------------|
| SizeWeight     | 0 < double < 1 [默认值：0.5]   | 允许控制泛型（高覆盖率）和信息性（多共享的）值之间的平衡。 如果增加该值，它通常会减少模式数量，并且每个模式所占覆盖率百分比可能会更大。 如果减少该值，它通常会产生更多特定模式，这些模式具有更多共享值，而覆盖率百分比会比较小。 该底层公式是加权几何平均值，介于规一化泛型分数和信息性分数之间，以 `SizeWeight` 和 `1-SizeWeight` 为权重                   | `T | evaluate autocluster(0.8)`                |
|WeightColumn    | column_name                     | 根据指定的权重考虑输入中的每一行（默认情况下每行具有权重“1”）。 该参数必须是数值列（如 int、long、real）的名称。 权重列的常见用法是对已嵌入每一行的数据进行采样或存储/聚合。                                                                                                       | `T | evaluate autocluster('~', sample_Count)` |
| NumSeeds        | int [默认值：25]              | 种子数决定算法初始本地搜索点的数量。 在有些情况下（具体取决于数据结构），如果增加种子数，那么，结果的数量（或质量）会由于搜索空间扩大而提高（代价是降低了查询速度）。 该值在两个方向上对结果的影响都会渐渐减少，因此，如果将它减小到 5 以下，它带来的性能改进将可以忽略不计。 如果增加到 50 以上，它将很难生成更多的模式。                                         | `T | evaluate autocluster('~', '~', 15)`       |
| CustomWildcard  | *"any_value_per_type"*           | 为结果表中的特定类型设置通配符值。 它将指示当前模式对此列没有限制。 默认值为 null，因为字符串默认值是一个空字符串。 如果默认值是数据中的正常值，则应使用其他通配符值（如 `*`）。                                                                                                                | `T | evaluate autocluster('~', '~', '~', '*', int(-1), double(-1), long(0), datetime(1900-1-1))` |

## <a name="examples"></a>示例

### <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents 
| where monthofyear(StartTime) == 5
| extend Damage = iff(DamageCrops + DamageProperty > 0 , "YES" , "NO")
| project State , EventType , Damage
| evaluate autocluster(0.6)
```

|段 ID|计数|百分比|状态|EventType|损害|
|---|---|---|---|---|---|---|---|---|
|0|2278|38.7||冰雹|是
|1|512|8.7||雷雨大风|YES
|2|898|15.3|德克萨斯||

### <a name="example-with-custom-wildcards"></a>使用自定义通配符的示例

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents 
| where monthofyear(StartTime) == 5
| extend Damage = iff(DamageCrops + DamageProperty > 0 , "YES" , "NO")
| project State , EventType , Damage 
| evaluate autocluster(0.2, '~', '~', '*')
```

|段 ID|计数|百分比|状态|EventType|损害|
|---|---|---|---|---|---|---|---|---|
|0|2278|38.7|\*|冰雹|是
|1|512|8.7|\*|雷雨大风|YES
|2|898|15.3|德克萨斯|\*|\*

## <a name="see-also"></a>另请参阅

* [basket](./basketplugin.md)
* [reduce](./reduceoperator.md)

* `autocluster` 主要基于以下文章中的 Seed-Expand 算法：[使用离散属性的遥测数据挖掘算法](https://www.scitepress.org/DigitalLibrary/PublicationsDetail.aspx?ID=d5kcrO+cpEU=&t=1)。 
