---
title: Azure Cosmos DB 查询语言中的 COT
description: 了解 Azure Cosmos DB 中的 Cotangent(COT) SQL 系统函数如何在指定的数值表达式中返回指定角度（以弧度为单位）的三角余切
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 98d284c0242441a8076c248d2d5529af80c5d9a8
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028673"
---
# <a name="cot-azure-cosmos-db"></a>COT (Azure Cosmos DB)
 返回指定数值表达式中指定角度的三角余切。  

## <a name="syntax"></a>语法

```sql
COT(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  为数值表达式。  

## <a name="return-types"></a>返回类型

  返回数值表达式。  

## <a name="examples"></a>示例

  以下示例计算指定角度的 `COT`。  

```sql
SELECT COT(124.1332) AS cot  
```  

 下面是结果集：  

```json
[{"cot": -0.040311998371148884}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->