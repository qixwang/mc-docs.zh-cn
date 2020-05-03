---
title: Azure Cosmos DB 查询语言中的 ABS
description: 了解 Azure Cosmos DB 中的 Absolute(ABS) SQL 系统函数如何返回指定数值表达式的正值
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/04/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: d7ac9fc7699e68cbdee49e5b6f8ba025264230d1
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134671"
---
# <a name="abs-azure-cosmos-db"></a>ABS (Azure Cosmos DB)
 返回指定数值表达式的绝对（正）值。  

## <a name="syntax"></a>语法

```sql
ABS (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例演示了对三个不同数字使用 `ABS` 函数的结果。  

```sql
SELECT ABS(-1) AS abs1, ABS(0) AS abs2, ABS(1) AS abs3 
```  

 下面是结果集。  

```json
[{abs1: 1, abs2: 0, abs3: 1}]  
```

## <a name="remarks"></a>备注

此系统函数将从[范围索引](index-policy.md#includeexclude-strategy)中获益。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->