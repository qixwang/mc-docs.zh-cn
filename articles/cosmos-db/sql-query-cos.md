---
title: Azure Cosmos DB 查询语言中的 COS
description: 了解 Azure Cosmos DB 中的 Cosine (COS) SQL 系统函数如何在指定表达式中返回指定角度（以弧度为单位）的三角余弦
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 605f98259c5594bdce63b3eec5a8354bb0b5459e
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028674"
---
# <a name="cos-azure-cosmos-db"></a>COS (Azure Cosmos DB)
 返回指定表达式中指定角度的三角余弦（弧度）。  

## <a name="syntax"></a>语法

```sql
COS(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例计算指定角度的 `COS`。  

```sql
SELECT COS(14.78) AS cos  
```  

 下面是结果集。  

```json
[{"cos": -0.59946542619465426}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->