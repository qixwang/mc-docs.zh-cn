---
title: Azure Cosmos DB 查询语言中的 CONTAINS
description: 了解 Azure Cosmos DB 中的 CONTAINS SQL 系统函数如何返回一个布尔值指示第一个字符串表达式是否包含第二个字符串表达式
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/03/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 63a8773af9401022d094fff61496499298edbb85
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134864"
---
# <a name="contains-azure-cosmos-db"></a>CONTAINS (Azure Cosmos DB)
 返回一个布尔值，该值指示第一个字符串表达式是否包含第二个字符串表达式。  

## <a name="syntax"></a>语法

```sql
CONTAINS(<str_expr1>, <str_expr2>)  
```  

## <a name="arguments"></a>参数

*str_expr1*  
  是要搜索的字符串表达式。  

*str_expr2*  
  是要查找的字符串表达式。  

## <a name="return-types"></a>返回类型

  返回一个布尔表达式。  

## <a name="examples"></a>示例

  以下示例检查“abc”是否包含“ab”以及“abc”是否包含“d”。  

```sql
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  

 下面是结果集。  

```json
[{"c1": true, "c2": false}]  
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->