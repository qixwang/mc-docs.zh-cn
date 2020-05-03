---
title: Azure Cosmos DB 查询语言中的 REPLACE
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 REPLACE。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: be1c66408dfc5bd97716d5f89acef63df9af7c60
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134762"
---
# <a name="replace-azure-cosmos-db"></a>REPLACE (Azure Cosmos DB)
 将出现的所有指定字符串值替换为另一个字符串值。  

## <a name="syntax"></a>语法

```sql
REPLACE(<str_expr1>, <str_expr2>, <str_expr3>)  
```  

## <a name="arguments"></a>参数

*str_expr1*  
  是要搜索的字符串表达式。  

*str_expr2*  
  要找到的字符串表达式。  

*str_expr3*  
  是用于替换 *str_expr1* 中的 *str_expr2* 匹配项的字符串表达式。  

## <a name="return-types"></a>返回类型

  返回一个字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `REPLACE`。  

```sql
SELECT REPLACE("This is a Test", "Test", "desk") AS replace 
```  

 下面是结果集。  

```json
[{"replace": "This is a desk"}]  
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->