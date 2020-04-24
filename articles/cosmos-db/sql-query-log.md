---
title: Azure Cosmos DB 查询语言中的 LOG
description: 了解 Azure Cosmos DB 中的 LOG SQL 系统函数，以便返回指定数值表达式的自然对数
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 5f416ee8ad2b18310dbf1fa6a69ea77e2ae3e4cd
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028965"
---
# <a name="log-azure-cosmos-db"></a>LOG (Azure Cosmos DB)
 返回指定数值表达式的自然对数。  

## <a name="syntax"></a>语法

```sql
LOG (<numeric_expr> [, <base>])  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  为数值表达式。  

base   
  设置对数底数的可选数值参数。  

## <a name="return-types"></a>返回类型

  返回数值表达式。  

## <a name="remarks"></a>备注

  默认情况下，LOG() 返回自然对数。 可以使用可选底数参数将对数的底数改为另一个值。  

  自然对数是底数为 e 的对数，其中 e 是一个无理数常数，约等于 2.718281828  **e**。 

  某个数的指数的自然对数就是该数本身：LOG( EXP( n ) ) = n。 并且某个数的自然对数的指数也是该数字本身：EXP( LOG( n ) ) = n。  

## <a name="examples"></a>示例

  以下示例声明一个变量并返回指定变量 (10) 的对数值。  

```sql
SELECT LOG(10) AS log  
```  

 下面是结果集：  

```json
[{log: 2.3025850929940459}]  
```  

 以下示例计算某数指数的 `LOG`。  

```sql
SELECT EXP(LOG(10)) AS expLog  
```  

 下面是结果集：  

```json
[{expLog: 10.000000000000002}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->