---
title: Azure Cosmos DB 查询语言中的 SIGN
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 SIGN。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 2558960d3da990f4fef4523bf3a35f5454afd6c5
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914668"
---
# <a name="sign-azure-cosmos-db"></a>SIGN (Azure Cosmos DB)
 返回指定数值表达式的正数 (+1)、零 (0) 或负数 (-1)。  

## <a name="syntax"></a>语法

```sql
SIGN(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  为数值表达式。  

## <a name="return-types"></a>返回类型

  返回数值表达式。  

## <a name="examples"></a>示例

  以下示例返回数字 -2 到 2 的 `SIGN` 值。 

```sql
SELECT SIGN(-2) AS s1, SIGN(-1) AS s2, SIGN(0) AS s3, SIGN(1) AS s4, SIGN(2) AS s5  
```  

 下面是结果集：  

```json
[{s1: -1, s2: -1, s3: 0, s4: 1, s5: 1}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query sign  -->
<!--New.date: 10/28/2019-->