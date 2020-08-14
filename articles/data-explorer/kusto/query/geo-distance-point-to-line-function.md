---
title: geo_distance_point_to_line() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 geo_distance_point_to_line()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: mbrichko
ms.service: data-explorer
ms.topic: reference
origin.date: 03/11/2020
ms.date: 08/06/2020
ms.openlocfilehash: c189596db6363d36240b09aa4846a045ac7eacbf
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841764"
---
# <a name="geo_distance_point_to_line"></a>geo_distance_point_to_line()

计算地球上坐标和直线之间的最短距离。

**语法**

`geo_distance_point_to_line(`longitude`, `latitude`, `lineString`)`  

**参数**

* longitude：地理空间坐标经度值（度）。 有效值为 [-180, +180] 范围内的实数。
* latitude：地理空间坐标纬度值（度）。 有效值为 [-90, +90] 范围内的实数。
* lineString：采用 [GeoJSON 格式](https://tools.ietf.org/html/rfc7946)的[动态](./scalar-data-types/dynamic.md)数据类型的直线。

**返回**

地球上坐标和直线之间的最短距离（米）。 如果坐标或 lineString 无效，则查询将生成 null 结果。

> [!NOTE]
> * 以 [WGS-84](https://earth-info.nga.mil/GandG/update/index.php?action=home) 坐标参考系统表示方法对地理空间坐标进行解释。
> * 用于测量地球上的距离的大地基准是一个球体。 直线边缘是球体上的测地线。

**LineString 定义和约束**

dynamic({"type":"LineString","coordinates": [ [lng_1,lat_1], [lng_2,lat_2] ,..., [lng_N,lat_N] ]})

* LineString 坐标数组必须至少包含两个条目。
* 如果 longitude 是 [-180, +180] 范围内的实数，而 latitude 是 [-90, +90] 范围内的实数，则坐标 [longitude,latitude] 必须有效。
* 边缘长度必须小于 180 度。 将选择两个顶点之间的最短边缘。

> [!TIP]
> 为了获得更好的性能，请使用文字直线。

**示例**

以下示例查找北拉斯维加斯机场和附近道路之间的最短距离。

:::image type="content" source="images/geo-distance-point-to-line-function/distance-point-to-line.png" alt-text="北拉斯维加斯机场与道路之间的距离":::

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print distance_in_meters = geo_distance_point_to_line(-115.199625, 36.210419, dynamic({ "type":"LineString","coordinates":[[-115.115385,36.229195],[-115.136995,36.200366],[-115.140252,36.192470],[-115.143558,36.188523],[-115.144076,36.181954],[-115.154662,36.174483],[-115.166431,36.176388],[-115.183289,36.175007],[-115.192612,36.176736],[-115.202485,36.173439],[-115.225355,36.174365]]}))
```

| distance_in_meters |
|--------------------|
| 3797.88887253334   |

美国南海岸的暴风雨事件。 按距离定义的海岸线最多 5 公里筛选事件。

:::image type="content" source="images/geo-distance-point-to-line-function/us-south-coast-storm-events.png" alt-text="美国南海岸的暴风雨事件":::

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
let southCoast = dynamic({"type":"LineString","coordinates":[[-97.18505859374999,25.997549919572112],[-97.58056640625,26.96124577052697],[-97.119140625,27.955591004642553],[-94.04296874999999,29.726222319395504],[-92.98828125,29.82158272057499],[-89.18701171875,29.11377539511439],[-89.384765625,30.315987718557867],[-87.5830078125,30.221101852485987],[-86.484375,30.4297295750316],[-85.1220703125,29.6880527498568],[-84.00146484374999,30.14512718337613],[-82.6611328125,28.806173508854776],[-82.81494140625,28.033197847676377],[-82.177734375,26.52956523826758],[-80.9912109375,25.20494115356912]]});
StormEvents
| project BeginLon, BeginLat, EventType
| where geo_distance_point_to_line(BeginLon, BeginLat, southCoast) < 5000
| render scatterchart with (kind=map) // map rendering available in Kusto Explorer desktop
```

纽约出租车接客。 按距离定义的直线最多 0.1 米筛选接客事件。

:::image type="content" source="images/geo-distance-point-to-line-function/park-ave-ny-road.png" alt-text="美国南海岸的暴风雨事件":::

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
nyc_taxi
| project pickup_longitude, pickup_latitude
| where geo_distance_point_to_line(pickup_longitude, pickup_latitude, dynamic({"type":"LineString","coordinates":[[-73.97583961486816,40.75486445336327],[-73.95215034484863,40.78743027428638]]})) <= 0.1
| take 50
| render scatterchart with (kind=map) // map rendering available in Kusto Explorer desktop
```

由于 LineString 输入无效，以下示例将返回 null 结果。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print distance_in_meters = geo_distance_point_to_line(1,1, dynamic({ "type":"LineString"}))
```

| distance_in_meters |
|--------------------|
|                    |

由于坐标输入无效，以下示例将返回 null 结果。

```kusto
print distance_in_meters = geo_distance_point_to_line(300, 3, dynamic({ "type":"LineString","coordinates":[[1,1],[2,2]]}))
```

| distance_in_meters |
|--------------------|
|                    |
