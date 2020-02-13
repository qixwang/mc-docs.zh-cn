---
title: Azure Cosmos DB 查询语言中的 CONCAT
description: 了解 Azure Cosmos DB 中的 CONCAT SQL 系统函数如何返回一个字符串来表示连接两个或更多字符串值的结果
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 89281084553bd516f7a5ce150f11e47f0cd30748
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028684"
---
# <a name="concat-azure-cosmos-db"></a>CONCAT (Azure Cosmos DB)
 返回一个字符串，该字符串是连接两个或多个字符串值的结果。  

## <a name="syntax"></a>语法

```sql
CONCAT(<str_expr1>, <str_expr2> [, <str_exprN>])  
```  

## <a name="arguments"></a>参数

*str_expr*  
  是要连接到其他值的字符串表达式。 `CONCAT` 函数需要至少两个 *str_expr* 参数。  

## <a name="return-types"></a>返回类型

  返回一个字符串表达式。  

## <a name="examples"></a>示例

  以下示例返回将指定值串联后形成的字符串。  

```sql
SELECT CONCAT("abc", "def") AS concat  
```  

 下面是结果集。  

```json
[{"concat": "abcdef"}]  
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->