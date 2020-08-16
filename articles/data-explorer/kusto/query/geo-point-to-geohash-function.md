---
title: geo_point_to_geohash() - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的 geo_point_to_geohash()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: mbrichko
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 08/06/2020
ms.openlocfilehash: 34186e73c1c518c1e28af2720a2e451810c7f539
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841203"
---
# <a name="geo_point_to_geohash"></a>geo_point_to_geohash()

计算地理位置的 geohash 字符串值。

**语法**

`geo_point_to_geohash(`*longitude*`, `*latitude*`, `[*accuracy*]`)`

**参数**

* longitude：地理位置的经度值。 如果 x 是实数且在 [-180, +180] 范围内，则经度 x 将被视为有效值。 
* latitude：地理位置的纬度值。 如果 y 是实数且在 [-90, +90] 范围内，则纬度 y 将被视为有效值。 
* accuracy：一个可选的 `int`，用于定义所请求的准确度。 支持的值范围为 [1,18]。 如果未指定，则使用默认值 `5`。

**返回**

其长度具有所请求的准确度的给定地理位置的 geohash 字符串值。 如果坐标或准确度无效，则查询会生成空结果。

> [!NOTE]
>
> * Geohash 是一个有用的地理空间聚类分析工具。
> * Geohash 有 18 个准确度级别，区域覆盖范围从最高级别 (1) 的 2500 万 km² 到最低级别 (18) 的 0.6 μ²。
> * geohash 的共有前缀表示点之间的靠近程度。 共享前缀越长，两个位置越靠近。 准确度值将转换为 geohash 长度。
> * Geohash 是平面表面上的一个矩形区域。
> * 对通过经度 x 和纬度 y 计算的 geohash 字符串调用 [geo_geohash_to_central_point()](geo-geohash-to-central-point-function.md) 函数不一定返回 x 和 y。
> * 根据 geohash 定义，有可能两个地理位置非常靠近，但有不同的 geohash 代码。

**每个准确度值对应的 Geohash 矩形区域覆盖范围：**

| 精确度 | 宽度     | 高度    |
|----------|-----------|-----------|
| 1        | 5000 km   | 5000 km   |
| 2        | 1250 km   | 625 km    |
| 3        | 156.25 km | 156.25 km |
| 4        | 39.06 km  | 19.53 km  |
| 5        | 4.88 km   | 4.88 km   |
| 6        | 1.22 km   | 0.61 km   |
| 7        | 152.59 m  | 152.59 m  |
| 8        | 38.15 m   | 19.07 m   |
| 9        | 4.77 m    | 4.77 m    |
| 10       | 1.19 m    | 0.59 m    |
| 11       | 149.01 mm | 149.01 mm |
| 12       | 37.25 mm  | 18.63 mm  |
| 13       | 4.66 mm   | 4.66 mm   |
| 14       | 1.16 mm   | 0.58 mm   |
| 15       | 145.52 μ  | 145.52 μ  |
| 16       | 36.28 μ   | 18.19 μ   |
| 17       | 4.55 μ    | 4.55 μ    |
| 18       | 1.14 μ    | 0.57 μ    |

另请参阅 [geo_point_to_s2cell()](geo-point-to-s2cell-function.md)。

**示例**

按 geohash 聚合的美国风暴事件。

:::image type="content" source="images/geo-point-to-geohash-function/geohash.png" alt-text="美国 geohash":::

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
StormEvents
| project BeginLon, BeginLat
| summarize by hash=geo_point_to_geohash(BeginLon, BeginLat, 3)
| project geo_geohash_to_central_point(hash)
| render scatterchart with (kind=map) // map rendering available in Kusto Explorer desktop
```

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print geohash = geo_point_to_geohash(139.806115, 35.554128, 12)  
```

| geohash      |
|--------------|
| xn76m27ty9g4 |

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print geohash = geo_point_to_geohash(-80.195829, 25.802215, 8)
```

|geohash|
|---|
|dhwfz15h|

以下示例查找坐标组。 组中的每对坐标都位于一个 4.88 km X 4.88 km 的矩形区域内。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(location_id:string, longitude:real, latitude:real)
[
  "A", double(-122.303404), 47.570482,
  "B", double(-122.304745), 47.567052,
  "C", double(-122.278156), 47.566936,
]
| summarize count = count(),                                          // items per group count
            locations = make_list(location_id)                        // items in the group
            by geohash = geo_point_to_geohash(longitude, latitude)    // geohash of the group
```

| geohash | 计数 | locations  |
|---------|-------|------------|
| c23n8   | 2     | ["A", "B"] |
| c23n9   | 1     | ["C"]      |

由于坐标输入无效，以下示例生成空结果。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print geohash = geo_point_to_geohash(200,1,8)
```

| geohash |
|---------|
|         |

由于准确度输入无效，以下示例生成空结果。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print geohash = geo_point_to_geohash(1,1,int(null))
```

| geohash |
|---------|
|         |
