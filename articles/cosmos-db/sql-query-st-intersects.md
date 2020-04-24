---
title: Azure Cosmos DB 查询语言中的 ST_INTERSECTS
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 ST_INTERSECTS。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 66e96f2065072202f2011eef8b51b12daf0e4f31
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914669"
---
# <a name="st_intersects-azure-cosmos-db"></a>ST_INTERSECTS (Azure Cosmos DB)
 返回一个布尔表达式，指示第一个参数中指定的 GeoJSON 对象（点、多边形或 LineString）是否与第二个参数中的 GeoJSON（点、多边形或 LineString）相交。  

## <a name="syntax"></a>语法

```sql
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  

## <a name="arguments"></a>参数

*spatial_expr*  
  是 GeoJSON 点、多边形或 LineString 对象表达式。  

## <a name="return-types"></a>返回类型

  返回一个布尔值。  

## <a name="examples"></a>示例

  以下示例介绍了如何查找与给定多边形相交的所有区域。  

```sql
SELECT a.id
FROM Areas a
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  

 下面是结果集：  

```json
[{ "id": "IntersectingPolygon" }]  
```  

## <a name="next-steps"></a>后续步骤

- [空间函数 Azure Cosmos DB](sql-query-spatial-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query st intersects  -->
<!--New.date: 10/28/2019-->