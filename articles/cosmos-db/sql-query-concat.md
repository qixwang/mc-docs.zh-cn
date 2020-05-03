---
title: Azure Cosmos DB 查询语言中的 CONCAT
description: 了解 Azure Cosmos DB 中的 CONCAT SQL 系统函数如何返回一个字符串来表示连接两个或更多字符串值的结果
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/03/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: fcd3f8210e4c34a9cfafefe56f54b21f8bbc8820
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134464"
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

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->