---
title: Azure Cosmos DB 查询语言中的 CEILING
description: 了解 Azure Cosmos DB 中的 CEILING SQL 系统函数如何返回大于或等于指定数值表达式的最小整数值。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: e5e544b40fa80ac1372dcb8fae1ef08f4751104f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028691"
---
# <a name="ceiling-azure-cosmos-db"></a>CEILING (Azure Cosmos DB)
 返回大于或等于指定数值表达式的最小整数值。  

## <a name="syntax"></a>语法

```sql
CEILING (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  为数值表达式。  

## <a name="return-types"></a>返回类型

  返回数值表达式。  

## <a name="examples"></a>示例

  以下示例显示了如何对正值、负值和零值使用 `CEILING` 函数。  

```sql
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  

 下面是结果集：  

```json
[{c1: 124, c2: -123, c3: 0}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->