---
title: geo_distance_2points() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 geo_distance_2points()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: mbrichko
ms.service: data-explorer
ms.topic: reference
origin.date: 03/11/2020
ms.date: 08/06/2020
ms.openlocfilehash: dd645894897fbd8e031aeec27f2f8abd6101ebc3
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841767"
---
# <a name="geo_distance_2points"></a>geo_distance_2points()

计算两个地理空间坐标在地球上的最短距离。

**语法**

`geo_distance_2points(`*p1_longitude*`, `*p1_latitude*`, `*p2_longitude*`, `*p2_latitude*`)`

**参数**

* p1_longitude：第一个地理空间坐标，经度值（度）。 有效值为 [-180, +180] 范围内的实数。
* p1_latitude：第一个地理空间坐标，纬度值（度）。 有效值为 [-90, +90] 范围内的实数。
* p2_longitude：第二个地理空间坐标，经度值（度）。 有效值为 [-180, +180] 范围内的实数。
* p2_latitude：第二个地理空间坐标，纬度值（度）。 有效值为 [-90, +90] 范围内的实数。

**返回**

地球上两个地理位置之间的最短距离（以米为单位）。 如果坐标无效，则查询将生成 null 结果。

> [!NOTE]
> * 对按照 [WGS-84](https://earth-info.nga.mil/GandG/update/index.php?action=home) 坐标参考系统表示的地理空间坐标进行解释。
> * 用来测量地球上距离的大地测量基准是一个球体。

**示例**

下面的示例查找西雅图与洛杉矶之间的最短距离。

:::image type="content" source="images/geo-distance-2points-function/distance_2points_seattle_los_angeles.png" alt-text="西雅图与洛杉矶之间的距离":::

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print distance_in_meters = geo_distance_2points(-122.407628, 47.578557, -118.275287, 34.019056)
```

| distance_in_meters |
|--------------------|
| 1546754.35197381   |

下面是从西雅图到伦敦的最短路径的近似值。 该线路由沿线串分布的距其 500 米范围内的坐标组成。

:::image type="content" source="images/geo-distance-2points-function/line_seattle_london.png" alt-text="西雅图到伦敦的线串":::

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
range i from 1 to 1000000 step 1
| project lng = rand() * real(-122), lat = rand() * 90
| where lng between(real(-122) .. 0) and lat between(47 .. 90)
| where geo_distance_point_to_line(lng,lat,dynamic({"type":"LineString","coordinates":[[-122,47],[0,51]]})) < 500
| render scatterchart with (kind=map) // map rendering available in Kusto Explorer desktop
```

以下示例查找其中的两个坐标之间的最短距离在 1 到 11 米之间的所有行。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
StormEvents
| extend distance_1_to_11m = geo_distance_2points(BeginLon, BeginLat, EndLon, EndLat)
| where distance_1_to_11m between (1 .. 11)
| project distance_1_to_11m
```

| distance_1_to_11m |
|-------------------|
| 10.5723100154958  |
| 7.92153588248414  |

下面的示例由于坐标输入无效而返回 null 结果。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print distance = geo_distance_2points(300,1,1,1)
```

| distance |
|----------|
|          |
