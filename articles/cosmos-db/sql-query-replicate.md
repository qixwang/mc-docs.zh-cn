---
title: Azure Cosmos DB 查询语言中的 REPLICATE
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 REPLICATE。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 4b5abd0d2c081f901370ebc578047e3ccd05900c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914682"
---
# <a name="replicate-azure-cosmos-db"></a>REPLICATE (Azure Cosmos DB)
 以指定的次数重复字符串值。

## <a name="syntax"></a>语法

```sql
REPLICATE(<str_expr>, <num_expr>)
```  

## <a name="arguments"></a>参数

*str_expr*  
  是一个字符串表达式。

*num_expr*  
  为数值表达式。 如果 *num_expr* 为负或非有限，则结果未定义。

## <a name="return-types"></a>返回类型

  返回字符串表达式。

## <a name="remarks"></a>备注
  结果的最大长度为 10,000 个字符，即 (length(*str_expr*)  *  *num_expr*) <= 10,000。

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `REPLICATE`。

```sql
SELECT REPLICATE("a", 3) AS replicate
```  

 下面是结果集：

```json
[{"replicate": "aaa"}]
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query replicate  -->
<!--New.date: 10/28/2019-->