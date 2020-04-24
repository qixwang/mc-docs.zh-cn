---
title: Azure Cosmos DB 查询语言中的 SIN
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 SIN。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: fa554efe8675314efd82724e5c58ec45b73bf466
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914664"
---
# <a name="sin-azure-cosmos-db"></a>SIN (Azure Cosmos DB)
 返回指定表达式中指定角度的三角正弦（弧度）。  

## <a name="syntax"></a>语法

```sql
SIN(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  为数值表达式。  

## <a name="return-types"></a>返回类型

  返回数值表达式。  

## <a name="examples"></a>示例

  以下示例计算指定角度的 `SIN`。  

```sql
SELECT SIN(45.175643) AS sin  
```  

 下面是结果集：  

```json
[{"sin": 0.929607286611012}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query sin  -->
<!--New.date: 10/28/2019-->