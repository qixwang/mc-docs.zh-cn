---
title: Azure Cosmos DB 查询语言中的 DEGREES
description: 了解 Azure Cosmos DB 中的 DEGREES SQL 系统函数，该函数返回指定角度（以弧度为单位）的相应角度（以度为单位）
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/03/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: d64fe4bd499cc5e31824dabeaae067c82cec4e7d
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134459"
---
# <a name="degrees-azure-cosmos-db"></a>DEGREES (Azure Cosmos DB)
 返回指定角度（弧度）的相应角度（度）。  

## <a name="syntax"></a>语法

```sql
DEGREES (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例返回 PI/2 弧度表示的角度的度数。  

```sql
SELECT DEGREES(PI()/2) AS degrees  
```  

 下面是结果集。  

```json
[{"degrees": 90}]  
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->