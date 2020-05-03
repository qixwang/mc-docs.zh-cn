---
title: Azure Cosmos DB 查询语言中的 TAN
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 TAN。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/04/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 6e92ce4bc887ac7d275ed26fa3b3bf51cca01cc8
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134788"
---
# <a name="tan-azure-cosmos-db"></a>TAN (Azure Cosmos DB)
 返回在指定表达式中以弧度表示的指定角度的正切。  

## <a name="syntax"></a>语法

```sql
TAN (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例计算 PI()/2 的正切。 

```sql
SELECT TAN(PI()/2) AS tan 
```  

 下面是结果集。  

```json
[{"tan": 16331239353195370 }]  
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->