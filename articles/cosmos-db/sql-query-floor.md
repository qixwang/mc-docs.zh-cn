---
title: Azure Cosmos DB 查询语言中的 FLOOR
description: 了解 Azure Cosmos DB 中的 FLOOR SQL 系统函数，该函数返回小于或等于指定数值表达式的最大整数
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 9571584ac477eec1d09eac19fae6046128fa5b12
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028660"
---
# <a name="floor-azure-cosmos-db"></a>FLOOR (Azure Cosmos DB)
 返回小于或等于指定数值表达式的最大整数。  

## <a name="syntax"></a>语法

```sql
FLOOR (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  为数值表达式。  

## <a name="return-types"></a>返回类型

  返回数值表达式。  

## <a name="examples"></a>示例

  以下示例显示了如何对正值、负值和零值使用 `FLOOR` 函数。  

```sql
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  

 下面是结果集：  

```json
[{fl1: 123, fl2: -124, fl3: 0}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->