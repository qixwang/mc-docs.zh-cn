---
title: Azure Cosmos DB 查询语言中的 TRUNC
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 TRUNC。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 3d67b9921f13a5afdf0aaa0207fe49b40d36c647
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134805"
---
# <a name="trunc-azure-cosmos-db"></a>TRUNC (Azure Cosmos DB)
 返回一个数值，截断到最接近的整数值。  

## <a name="syntax"></a>语法

```sql
TRUNC(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
   是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例将以下正数和负数截断到最接近的整数值。  

```sql
SELECT TRUNC(2.4) AS t1, TRUNC(2.6) AS t2, TRUNC(2.5) AS t3, TRUNC(-2.4) AS t4, TRUNC(-2.6) AS t5  
```  

 下面是结果集。  

```json
[{t1: 2, t2: 2, t3: 2, t4: -2, t5: -2}]  
```

## <a name="remarks"></a>备注

此系统函数将从[范围索引](index-policy.md#includeexclude-strategy)中获益。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->