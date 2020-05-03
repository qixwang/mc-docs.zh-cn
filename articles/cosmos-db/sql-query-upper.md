---
title: Azure Cosmos DB 查询语言中的 UPPER
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 UPPER。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/04/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: ff0e76e4920824208e7fd0ff277b630dd7c14550
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134798"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
 返回在将小写字符数据转换为大写后的字符串表达式。  

UPPER 系统函数不使用索引。 如果计划频繁进行不区分大小写的比较，则 UPPER 系统函数可能会消耗大量 RU。 如果是这种情况，可以在插入时将大小写规范化，而不是每次都使用 UPPER 系统函数将数据规范化以进行比较。 于是，诸如 SELECT * FROM c WHERE UPPER(c.name) = 'BOB' 的查询只需变成 SELECT * FROM c WHERE c.name = 'BOB'。

## <a name="syntax"></a>语法

```sql
UPPER(<str_expr>)  
```  

## <a name="arguments"></a>参数

*str_expr*  
  是一个字符串表达式。  

## <a name="return-types"></a>返回类型

  返回一个字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `UPPER`  

```sql
SELECT UPPER("Abc") AS upper  
```  

 下面是结果集。  

```json
[{"upper": "ABC"}]  
```

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->