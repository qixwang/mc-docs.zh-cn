---
title: Azure Cosmos DB 查询语言中的 RIGHT
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 RIGHT。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 2cd1357d69da2de976bc65866b245fa488fec627
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914680"
---
# <a name="right-azure-cosmos-db"></a>RIGHT (Azure Cosmos DB)
 返回具有指定字符数的字符串的右侧部分。  

## <a name="syntax"></a>语法

```sql
RIGHT(<str_expr>, <num_expr>)  
```  

## <a name="arguments"></a>参数

*str_expr*  
  要从中提取字符的字符串表达式。  

*num_expr*  
  是指定字符数的数值表达式。  

## <a name="return-types"></a>返回类型

  返回字符串表达式。  

## <a name="examples"></a>示例

  以下示例将返回不同长度值的 "abc" 的右侧部分。  

```sql
SELECT RIGHT("abc", 1) AS r1, RIGHT("abc", 2) AS r2 
```  

 下面是结果集：  

```json
[{"r1": "c", "r2": "bc"}]  
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query right  -->
<!--New.date: 10/28/2019-->