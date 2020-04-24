---
title: Azure Cosmos DB 查询语言中的 ST_WITHIN
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 ST_WITHIN。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 72ddd306a08345ad4e247ec70fc098a1892702dc
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914654"
---
# <a name="st_within-azure-cosmos-db"></a>ST_WITHIN (Azure Cosmos DB)
 返回一个布尔表达式，指示第一个参数中指定的 GeoJSON 对象（点、多边形或 LineString）是否在第二个参数中的 GeoJSON（点、多边形或 LineString）内。  

## <a name="syntax"></a>语法

```sql
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  

## <a name="arguments"></a>参数

*spatial_expr*  
  是 GeoJSON 点、多边形或 LineString 对象表达式。  

## <a name="return-types"></a>返回类型

  返回一个布尔值。  

## <a name="examples"></a>示例

  以下示例演示了如何使用 `ST_WITHIN` 查找某个多边形内的所有家族文档。  

```sql
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  

 下面是结果集：  

```json
[{ "id": "WakefieldFamily" }]  
```  

## <a name="next-steps"></a>后续步骤

- [空间函数 Azure Cosmos DB](sql-query-spatial-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query st within  -->
<!--New.date: 10/28/2019-->