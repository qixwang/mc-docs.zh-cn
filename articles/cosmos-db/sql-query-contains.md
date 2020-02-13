---
title: Azure Cosmos DB 查询语言中的 CONTAINS
description: 了解 Azure Cosmos DB 中的 CONTAINS SQL 系统函数如何返回一个布尔值指示第一个字符串表达式是否包含第二个字符串表达式
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 2800ef4500d417caff12bcf7c9ba02f2e7530754
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028678"
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

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->