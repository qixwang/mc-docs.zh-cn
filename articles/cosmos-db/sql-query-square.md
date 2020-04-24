---
title: Azure Cosmos DB 查询语言中的 SQUARE
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 SQUARE。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 3ca9573e1f25a464e5245ec27187568c371d5d5e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914660"
---
# <a name="square-azure-cosmos-db"></a>SQUARE (Azure Cosmos DB)
 返回指定数字值的平方。  

## <a name="syntax"></a>语法

```sql
SQUARE(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  为数值表达式。  

## <a name="return-types"></a>返回类型

  返回数值表达式。  

## <a name="examples"></a>示例

  以下示例将返回数字 1-3 的平方。 

```sql
SELECT SQUARE(1) AS s1, SQUARE(2.0) AS s2, SQUARE(3) AS s3  
```  

 下面是结果集：  

```json
[{s1: 1, s2: 4, s3: 9}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query square  -->
<!--New.date: 10/28/2019-->