---
title: Azure Cosmos DB 查询语言中的 ARRAY_SLICE
description: 了解 Azure Cosmos DB 中的 Array slice SQL 系统函数如何返回部分数组表达式
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 1480093d8be6a01948eb3e70fe769c068d7aaaab
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028699"
---
# <a name="array_slice-azure-cosmos-db"></a>ARRAY_SLICE (Azure Cosmos DB)
 返回部分数组表达式。

## <a name="syntax"></a>语法

```sql
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  

## <a name="arguments"></a>参数

*arr_expr*  
  是任何数组表达式。  

*num_expr*  
   用于开始数组的从零开始的数字索引。 负值可用于指定相对于数组最后一个元素的起始索引，即 -1 引用数组中最后一个元素。  

*num_expr* 可选数值表达式，用于设置生成的数组中的最大元素数。    

## <a name="return-types"></a>返回类型

  返回数组表达式。  

## <a name="examples"></a>示例

  以下示例演示了如何使用 `ARRAY_SLICE` 获取数组的不同切片。  

```sql
SELECT
   ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
   ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
   ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
   ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
   ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
   ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
   ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      

```  

 下面是结果集：  

```json
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  

## <a name="next-steps"></a>后续步骤

- [数组函数 Azure Cosmos DB](sql-query-array-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update -->