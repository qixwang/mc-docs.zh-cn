---
title: geo_s2cell_to_central_point() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 geo_s2cell_to_central_point()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: mbrichko
ms.service: data-explorer
ms.topic: reference
origin.date: 01/27/2020
ms.date: 08/06/2020
ms.openlocfilehash: 8e7e2e998c94a02910bbc9a379f5d1ed67ea84f5
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841753"
---
# <a name="geo_s2cell_to_central_point"></a>geo_s2cell_to_central_point()

计算表示 S2 单元格中心的地理空间坐标。

详细了解 [S2 单元格层次结构](https://s2geometry.io/devguide/s2cell_hierarchy)。

**语法**

`geo_s2cell_to_central_point(`*s2cell*`)`

**参数**

s2cell：由 [geo_point_to_s2cell()](geo-point-to-s2cell-function.md) 计算得出的 S2 单元格令牌字符串值。 S2 单元格令牌的最大字符串长度为 16 个字符。

**返回**

地理空间坐标值采用 [GeoJSON 格式](https://tools.ietf.org/html/rfc7946)，属于[动态](./scalar-data-types/dynamic.md)数据类型。 如果 S2 单元格令牌无效，则查询会生成 null 结果。

> [!NOTE]
> GeoJSON 格式先指定经度，再指定纬度。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print point = geo_s2cell_to_central_point("1234567")
| extend coordinates = point.coordinates
| extend longitude = coordinates[0], latitude = coordinates[1]
```

|point|坐标|longitude|latitude|
|---|---|---|---|
|{<br>  "type":"Point",<br>  "coordinates": [<br>    9.86830731850408,<br>    27.468392925827604<br>  ]<br>}|[<br>  9.86830731850408,<br>  27.468392925827604<br>]|9.86830731850408|27.4683929258276|

下面的示例由于 S2 单元格令牌输入无效而返回 null 结果。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print point = geo_s2cell_to_central_point("a")
```

|point|
|---|
||
