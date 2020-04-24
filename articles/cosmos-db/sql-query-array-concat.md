---
title: Azure Cosmos DB 查询语言中的 ARRAY_CONCAT
description: 了解 Azure Cosmos DB 中的 Array Concat SQL 系统函数如何返回一个数组来表示连接两个或更多数组值的结果
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: ccbee0862b68df5ded411150ae4446097f7c48de
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028708"
---
# <a name="array_concat-azure-cosmos-db"></a>ARRAY_CONCAT (Azure Cosmos DB)
 返回一个数组，该数组是连接两个或更多数组值的结果。  

## <a name="syntax"></a>语法

```sql
ARRAY_CONCAT (<arr_expr1>, <arr_expr2> [, <arr_exprN>])  
```  

## <a name="arguments"></a>参数

*arr_expr*  
  是要连接到其他值的数组表达式。 `ARRAY_CONCAT` 函数需要至少两个 *arr_expr* 参数。  

## <a name="return-types"></a>返回类型

  返回数组表达式。  

## <a name="examples"></a>示例

  以下示例介绍了如何连接两个数组。  

```sql
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  

 下面是结果集：  

```json
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  

## <a name="next-steps"></a>后续步骤

- [数组函数 Azure Cosmos DB](sql-query-array-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->
