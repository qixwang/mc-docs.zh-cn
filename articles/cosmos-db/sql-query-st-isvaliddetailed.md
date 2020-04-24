---
title: Azure Cosmos DB 查询语言中的 ST_ISVALIDDETAILED
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 ST_ISVALIDDETAILED。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 349bcff200b21980d31535ec7d8b237079923fe1
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914659"
---
# <a name="st_isvaliddetailed-azure-cosmos-db"></a>ST_ISVALIDDETAILED (Azure Cosmos DB)
 如果指定的 GeoJSON 点、多边形或 LineString 表达式有效，则返回包含布尔值的 JSON 值；如果无效，则额外加上作为字符串值的原因。  

## <a name="syntax"></a>语法

```sql
ST_ISVALIDDETAILED(<spatial_expr>)  
```  

## <a name="arguments"></a>参数

*spatial_expr*  
  是 GeoJSON 点或多边形表达式。  

## <a name="return-types"></a>返回类型

  如果指定的 GeoJSON 点或多边形表达式有效，则返回包含布尔值的 JSON 值；如果无效，则额外添加原因作为字符串值。  

## <a name="examples"></a>示例

  以下示例演示了如何使用 `ST_ISVALIDDETAILED` 检查有效性（带详细信息）。  

```sql
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b 
```  

 下面是结果集：  

```json
[{  
  "b": {
    "valid": false,
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  

## <a name="next-steps"></a>后续步骤

- [空间函数 Azure Cosmos DB](sql-query-spatial-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query st isvaliddetailed  -->
<!--New.date: 10/28/2019-->