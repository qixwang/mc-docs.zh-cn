---
title: Azure Cosmos DB 查询语言中的 DEGREES
description: 了解 Azure Cosmos DB 中的 DEGREES SQL 系统函数，该函数返回指定角度（以弧度为单位）的相应角度（以度为单位）
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: f3396a21ac4b23d35978169db71301fd0debec02
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028667"
---
# <a name="degrees-azure-cosmos-db"></a>DEGREES (Azure Cosmos DB)
 返回指定角度（弧度）的相应角度（度）。  

## <a name="syntax"></a>语法

```sql
DEGREES (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例返回 PI/2 弧度表示的角度的度数。  

```sql
SELECT DEGREES(PI()/2) AS degrees  
```  

 下面是结果集。  

```json
[{"degrees": 90}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->