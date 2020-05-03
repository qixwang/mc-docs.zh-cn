---
title: Azure Cosmos DB 查询语言中的 STARTSWITH
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 STARTSWITH。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 8c522e1b4e1dd9314b7ad5d63ac1e9305e1a8f82
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134832"
---
# <a name="startswith-azure-cosmos-db"></a>STARTSWITH (Azure Cosmos DB)
 返回一个布尔值，指示第一个字符串表达式是否以第二个字符串表达式开头。  

## <a name="syntax"></a>语法

```sql
STARTSWITH(<str_expr1>, <str_expr2>)  
```  

## <a name="arguments"></a>参数

*str_expr1*  
  是一个字符串表达式。

*str_expr2*  
  是要与 *str_expr1* 的开头进行比较的字符串表达式。

## <a name="return-types"></a>返回类型

  返回一个布尔表达式。  

## <a name="examples"></a>示例

  以下示例检查字符串“abc”是否以“b”和“a”开头。  

```sql
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  

 下面是结果集。  

```json
[{"s1": false, "s2": true}]  
```  

## <a name="remarks"></a>备注

此系统函数将从[范围索引](index-policy.md#includeexclude-strategy)中获益。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->