---
title: Azure Cosmos DB 查询语言中的 TRIM
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 TRIM。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/04/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: fc6737221d62e893aa3f285c0c8457b8edbc0a66
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134777"
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

  返回一个字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `TRIM`。  

```sql
SELECT TRIM("   abc") AS t1, TRIM("   abc   ") AS t2, TRIM("abc   ") AS t3, TRIM("abc") AS t4
```  

 下面是结果集。  

```json
[{"t1": "abc", "t2": "abc", "t3": "abc", "t4": "abc"}]  
``` 

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->