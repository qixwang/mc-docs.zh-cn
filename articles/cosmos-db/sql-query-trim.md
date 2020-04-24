---
title: Azure Cosmos DB 查询语言中的 TRIM
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 TRIM。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 8d91794dfa5f2dd57b6847f7b35a529d4be13a12
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914631"
---
# <a name="trim-azure-cosmos-db"></a>TRIM (Azure Cosmos DB)
 返回删除前导空格和尾随空格后的字符串表达式。  

## <a name="syntax"></a>语法

```sql
TRIM(<str_expr>)  
```  

## <a name="arguments"></a>参数

*str_expr*  
  是一个字符串表达式。  

## <a name="return-types"></a>返回类型

  返回字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `TRIM`。  

```sql
SELECT TRIM("   abc") AS t1, TRIM("   abc   ") AS t2, TRIM("abc   ") AS t3, TRIM("abc") AS t4
```  

 下面是结果集：  

```json
[{"t1": "abc", "t2": "abc", "t3": "abc", "t4": "abc"}]  
``` 

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query trim  -->
<!--New.date: 10/28/2019-->