---
title: geo_point_to_s2cell() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 geo_point_to_s2cell()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: mbrichko
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 08/06/2020
ms.openlocfilehash: 8878574c5f6f86b5463bb25d8668e6a058f26821
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841756"
---
# <a name="geo_point_to_s2cell"></a>geo_point_to_s2cell()

计算地理位置的 S2 单元格字符串值。

详细了解 [S2 单元格层次结构](https://s2geometry.io/devguide/s2cell_hierarchy)。

**语法**

`geo_point_to_s2cell(`*longitude*`, `*latitude*`, `*level*`)`

**参数**

* *经度*：地理位置的经度值。 如果 x 是实数且在 [-180, +180] 范围内，则经度 x 将被视为有效  。 
* *纬度*：地理位置的纬度值。 如果 y 是实数且在 [-90, +90] 范围内，则纬度 y 将被视为有效。 
* *级别*：可选的 `int`，用于定义请求的单元格级别。 支持的值范围为 [0, 30]。 如果未指定，则使用默认值 `11`。

**返回**

给定地理位置的 S2 单元格令牌字符串值。 如果坐标或级别无效，则查询将生成空结果。

> [!NOTE]
>
> * S2 单元格是一个有用的地理空间聚类分析工具。
> * S2 单元格共有 31 个级别，面积从最高 0 层的 85,011,012.19km² 到最低 30 层的 00.44cm²。
> * S2 单元格在级别从 0 增加到 30 时，单元格中心保持良好。
> * S2 单元格是球面上的一个单元，它的边缘是测地线。
> * 对通过经度 x 和纬度 y 计算的 S2 单元格令牌字符串调用 [geo_s2cell_to_central_point()](geo-s2cell-to-central-point-function.md) 函数，不一定返回 x 和 y。
> * 有可能两个地理位置非常接近，但有不同的 S2 单元格标记。

**每个级别值的 S2 单元格近似面积覆盖率**

对于每个级别，S2 单元格的大小相似，但不完全相等。 附近的单元格大小趋于相等。

|级别|最小随机单元格边缘长度（英国）|最大随机单元格边缘长度（美国）|
|--|--|--|
|0|7842 km|7842 km|
|1|3921 km|5004 km|
|2|1825 km|2489 km|
|3|840 km|1310 km|
|4|432 km|636 km|
|5|210 km|315 km|
|6|108 km|156 km|
|7|54 km|78 km|
|8|27 km|39 km|
|9|14 km|20 km|
|10|7 km|10 km|
|11|3 km|5 km|
|12|1699 m|2 km|
|13|850 m|1225 m|
|14|425 m|613 m|
|15|212 m|306 m|
|16|106 m|153 m|
|17|53 m|77 m|
|18|27 m|38 m|
|19|13 m|19 m|
|20|7 m|10 m|
|21|3 m|5 m|
|22|166 cm|2 m|
|23|83 cm|120 cm|
|24|41 cm|60 cm|
|25|21 cm|30 cm|
|26|10 cm|15 cm|
|27|5 cm|7 cm|
|28|2 cm|4 cm|
|29|12 mm|18 mm|
|30|6 mm|9 mm|

表源可以在[此 S2 单元格统计资源](https://s2geometry.io/resources/s2cell_statistics)中找到。

另请参阅 [geo_point_to_geohash()](geo-point-to-geohash-function.md)。

**示例**

由 s2cell 聚合的美国风暴事件。

:::image type="content" source="images/geo-point-to-s2cell-function/s2cell.png" alt-text="美国 s2cell":::

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
StormEvents
| project BeginLon, BeginLat
| summarize by hash=geo_point_to_s2cell(BeginLon, BeginLat, 5)
| project geo_s2cell_to_central_point(hash)
| render scatterchart with (kind=map) // map rendering available in Kusto Explorer desktop
```

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print s2cell = geo_point_to_s2cell(-80.195829, 25.802215, 8)
```

| s2cell |
|--------|
| 88d9b  |

以下示例查找坐标组。 组中的每对坐标都位于 S2 单元格中，最大面积为 1632.45 km²。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(location_id:string, longitude:real, latitude:real)
[
  "A", 10.1234, 53,
  "B", 10.3579, 53,
  "C", 10.6842, 53,
]
| summarize count = count(),                                        // items per group count
            locations = make_list(location_id)                      // items in the group
            by s2cell = geo_point_to_s2cell(longitude, latitude, 8) // s2 cell of the group
```

| s2cell | 计数 | locations |
|--------|-------|-----------|
| 47b1d  | 2     | ["A","B"] |
| 47ae3  | 1     | ["C"]     |

由于坐标输入无效，以下示例生成空结果。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print s2cell = geo_point_to_s2cell(300,1,8)
```

| s2cell |
|--------|
|        |

由于级别输入无效，以下示例生成空结果。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print s2cell = geo_point_to_s2cell(1,1,35)
```

| s2cell |
|--------|
|        |

由于级别输入无效，以下示例生成空结果。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print s2cell = geo_point_to_s2cell(1,1,int(null))
```

| s2cell |
|--------|
|        |
