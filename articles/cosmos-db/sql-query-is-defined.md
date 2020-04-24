---
title: Azure Cosmos DB 查询语言中的 IS_DEFINED
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 IS_DEFINED。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 8424b5fcb59716c335dcba6b30cb1fc8469719fd
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914722"
---
# <a name="is_defined-azure-cosmos-db"></a>IS_DEFINED (Azure Cosmos DB)
 返回一个布尔，它指示属性是否已经分配了值。  

## <a name="syntax"></a>语法

```sql
IS_DEFINED(<expr>)  
```  

## <a name="arguments"></a>参数

*expr*  
  是任何表达式。  

## <a name="return-types"></a>返回类型

  返回一个布尔表达式。  

## <a name="examples"></a>示例

  以下示例检查了指定的 JSON 文档内是否存在属性。 由于存在 "a"，第一个会返回 true，但由于不存在 "b"，第二个会返回 false。  

```sql
SELECT IS_DEFINED({ "a" : 5 }.a) AS isDefined1, IS_DEFINED({ "a" : 5 }.b) AS isDefined2 
```  

 下面是结果集：  

```json
[{"isDefined1":true,"isDefined2":false}]  
```  

## <a name="next-steps"></a>后续步骤

- [类型检查函数 Azure Cosmos DB](sql-query-type-checking-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query is defined -->
<!--New.date: 10/28/2019-->