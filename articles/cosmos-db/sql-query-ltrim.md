---
title: Azure Cosmos DB 查询语言中的 LTRIM
description: 了解 Azure Cosmos DB 中的 LTRIM SQL 系统函数，以便返回删除前导空格后的字符串表达式。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 8c7c276655d276ff14ef880acf1290d0d95663f1
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134546"
---
# <a name="ltrim-azure-cosmos-db"></a>LTRIM (Azure Cosmos DB)
 返回删除前导空格后的字符串表达式。  

## <a name="syntax"></a>语法

```sql
LTRIM(<str_expr>)  
```  

## <a name="arguments"></a>参数

*str_expr*  
  是一个字符串表达式。  

## <a name="return-types"></a>返回类型

  返回一个字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `LTRIM`。  

```sql
SELECT LTRIM("  abc") AS l1, LTRIM("abc") AS l2, LTRIM("abc   ") AS l3 
```  

 下面是结果集。  

```json
[{"l1": "abc", "l2": "abc", "l3": "abc   "}]  
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->