---
title: Azure Cosmos DB 查询语言中的 RTRIM
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 RTRIM。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 72184d97917280f54dc15b6b9f4c088014f8eac8
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914678"
---
# <a name="rtrim-azure-cosmos-db"></a>RTRIM (Azure Cosmos DB)
 在删除尾随空格后，返回字符串表达式。  

## <a name="syntax"></a>语法

```sql
RTRIM(<str_expr>)  
```  

## <a name="arguments"></a>参数

*str_expr*  
  为任何有效的字符串表达式。  

## <a name="return-types"></a>返回类型

  返回字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `RTRIM`。  

```sql
SELECT RTRIM("  abc") AS r1, RTRIM("abc") AS r2, RTRIM("abc   ") AS r3  
```  

 下面是结果集：  

```json
[{"r1": "   abc", "r2": "abc", "r3": "abc"}]  
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query rtrim  -->
<!--New.date: 10/28/2019-->