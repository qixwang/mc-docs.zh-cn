---
title: Azure Cosmos DB 查询语言中的 ST_DISTANCE
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 ST_DISTANCE。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 89d3a45446f5dcadf91709618b9c5677b9120034
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914661"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
 返回两个 GeoJSON 点、多边形或 LineString 表达式之间的距离。  

## <a name="syntax"></a>语法

```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  

## <a name="arguments"></a>参数

*spatial_expr*  
  为任何有效的 GeoJSON 点、多边形或 LineString 对象表达式。  

## <a name="return-types"></a>返回类型

  返回包含距离的数值表达式。 默认参考系统以米为单位表示。  

## <a name="examples"></a>示例

  以下示例演示了如何使用 `ST_DISTANCE` 内置函数返回与指定位置的距离在 30 公里内的所有家族文档。 。  

```sql
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  

 下面是结果集：  

```json
[{  
  "id": "WakefieldFamily"  
}]  
```  

## <a name="next-steps"></a>后续步骤

- [空间函数 Azure Cosmos DB](sql-query-spatial-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query st distance  -->
<!--New.date: 10/28/2019-->