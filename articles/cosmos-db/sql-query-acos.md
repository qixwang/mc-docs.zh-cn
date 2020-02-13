---
title: Azure Cosmos DB 查询语言中的 ACOS
description: 了解 Azure Cosmos DB 中的 ACOS (arccosice) SQL 系统函数如何返回其余弦是指定数值表达式的角度（以弧度为单位）
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: e7b42fb5fcd79bb0852ca0471c1ea99ea2f2de9e
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028718"
---
# <a name="acos-azure-cosmos-db"></a>ACOS (Azure Cosmos DB)
 返回角度（弧度），其余弦是指定的数值表达式；也被称为反余弦。  

## <a name="syntax"></a>语法

```sql
ACOS(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例返回 -1 的 `ACOS`。 

```sql
SELECT ACOS(-1) AS acos 
```  

 下面是结果集。  

```json
[{"acos": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->