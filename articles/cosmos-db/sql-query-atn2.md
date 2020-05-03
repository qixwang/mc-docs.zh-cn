---
title: Azure Cosmos DB 查询语言中的 ATN2
description: 了解 Azure Cosmos DB 中的 ATN2 SQL 系统函数如何返回 y/x 的反正切的主体值（以弧度表示）。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/03/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 75040c7d99ac1bcadba61164e20511bb5fbf9f03
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134747"
---
# <a name="atn2-azure-cosmos-db"></a>ATN2 (Azure Cosmos DB)
 返回 y/x 的反正切的主体值，以弧度表示。  

## <a name="syntax"></a>语法

```sql
ATN2(<numeric_expr>, <numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例为指定的 x 和 y 组件计算 ATN2。  

```sql
SELECT ATN2(35.175643, 129.44) AS atn2  
```  

 下面是结果集。  

```json
[{"atn2": 1.3054517947300646}]  
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->