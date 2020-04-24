---
title: Azure Cosmos DB 查询语言中的 STARTSWITH
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 STARTSWITH。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 372956bffa3a257d31464b775e535466358e4e8a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914647"
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

  以下示例检查了字符串 "abc" 是否以 "b" 和 "a" 开头。  

```sql
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  

 下面是结果集：  

```json
[{"s1": false, "s2": true}]  
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query startswith  -->
<!--New.date: 10/28/2019-->