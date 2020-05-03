---
title: Azure Cosmos DB 查询语言中的 ROUND
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 ROUND。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 7776677544d5bcf09210ed27fa3f72a1d0a077ef
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134755"
---
# <a name="round-azure-cosmos-db"></a>ROUND (Azure Cosmos DB)
 返回一个数值，四舍五入到最接近的整数值。  

## <a name="syntax"></a>语法

```sql
ROUND(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="remarks"></a>备注

执行的四舍五入运算遵循远离零的中点四舍五入。 如果输入是正好介于两个整数之间的数值表达式，则结果将是离零最近的整数值。  

  |<numeric_expr>|已四舍五入|
  |-|-|
  |-6.5000|-7|
  |-0.5|-1|
  |0.5|1|
  |6.5000|7||

## <a name="examples"></a>示例

以下示例将以下正数和负数舍入到最接近的整数。  

```sql
SELECT ROUND(2.4) AS r1, ROUND(2.6) AS r2, ROUND(2.5) AS r3, ROUND(-2.4) AS r4, ROUND(-2.6) AS r5  
```  

  下面是结果集。  

```json
[{r1: 2, r2: 3, r3: 3, r4: -2, r5: -3}]  
```  

## <a name="remarks"></a>备注

此系统函数将从[范围索引](index-policy.md#includeexclude-strategy)中获益。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->