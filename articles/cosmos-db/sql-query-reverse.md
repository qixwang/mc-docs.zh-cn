---
title: Azure Cosmos DB 查询语言中的 REVERSE
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 REVERSE。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: ca02b053647492e05471796d1af935c3e3a526c9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914683"
---
# <a name="reverse-azure-cosmos-db"></a>REVERSE (Azure Cosmos DB)
 返回字符串值的逆序。  

## <a name="syntax"></a>语法

```sql
REVERSE(<str_expr>)  
```  

## <a name="arguments"></a>参数

*str_expr*  
  是一个字符串表达式。  

## <a name="return-types"></a>返回类型

  返回字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `REVERSE`。  

```sql
SELECT REVERSE("Abc") AS reverse  
```  

 下面是结果集：  

```json
[{"reverse": "cbA"}]  
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query reverse  -->
<!--New.date: 10/28/2019-->