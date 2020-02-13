---
title: Azure Cosmos DB 查询语言中的 ARRAY_LENGTH
description: 了解 Azure Cosmos DB 中的 Array length SQL 系统函数如何返回指定数组表达式的元素数
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: e183b0a14a376d74065fabfdcc7a7c7248580087
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028700"
---
# <a name="array_length-azure-cosmos-db"></a>ARRAY_LENGTH (Azure Cosmos DB)
 返回指定数组表达式的元素数。  

## <a name="syntax"></a>语法

```sql
ARRAY_LENGTH(<arr_expr>)  
```  

## <a name="arguments"></a>参数

*arr_expr*  
  是一个数组表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例演示了如何使用 `ARRAY_LENGTH` 获取数组的长度。  

```sql
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  

 下面是结果集。  

```json
[{"len": 3}]  
```  

## <a name="next-steps"></a>后续步骤

- [数组函数 Azure Cosmos DB](sql-query-array-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update -->