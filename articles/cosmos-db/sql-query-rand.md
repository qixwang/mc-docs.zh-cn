---
title: Azure Cosmos DB 查询语言中的 RAND
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 RAND。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/16/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 1557652d2adee3f7c828e24692ceb282d4067810
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914684"
---
# <a name="rand-azure-cosmos-db"></a>RAND (Azure Cosmos DB)
 返回 [0,1) 中随机生成的数值。

## <a name="syntax"></a>语法

```sql
RAND ()  
```  

## <a name="return-types"></a>返回类型

  返回数值表达式。

## <a name="remarks"></a>备注

  `RAND` 是非确定性函数。 重复调用 `RAND` 不会返回相同的结果。

## <a name="examples"></a>示例

  以下示例返回一个随机生成的数值。

```sql
SELECT RAND() AS rand 
```  

 下面是结果集：  

```json
[{"rand": 0.87860053195618093}]  
``` 

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query rand  -->
<!--New.date: 10/28/2019-->