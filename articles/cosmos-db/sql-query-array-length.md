---
title: Azure Cosmos DB 查询语言中的 ARRAY_LENGTH
description: 了解 Azure Cosmos DB 中的 Array length SQL 系统函数如何返回指定数组表达式的元素数
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/03/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 2cf31e85caeee4a6a681c63247d59ce4ad9145a9
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134567"
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

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [数组函数 Azure Cosmos DB](sql-query-array-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->