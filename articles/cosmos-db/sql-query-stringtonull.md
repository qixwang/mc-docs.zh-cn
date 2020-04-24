---
title: Azure Cosmos DB 查询语言中的 StringToNull
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 StringToNull。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 6906e66d73726fe5b9e9f58f6938ee5d2e39bb49
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914642"
---
# <a name="stringtonull-azure-cosmos-db"></a>StringToNull (Azure Cosmos DB)
 返回已转换为 Null 的表达式。 如果表达式无法转换，则返回未定义的表达式。  

## <a name="syntax"></a>语法

```sql
StringToNull(<str_expr>)  
```  

## <a name="arguments"></a>参数

*str_expr*  
  是要解析为 null 表达式的字符串表达式。

## <a name="return-types"></a>返回类型

  返回一个 Null 表达式或未定义的表达式。  

## <a name="examples"></a>示例

  以下示例演示 `StringToNull` 在不同类型中的行为方式。 

下面是输入有效的示例。

 只能在 "null" 之前或之后使用空格。

```sql
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  

 下面是结果集：  

```json
[{"n1": null, "n2": null, "n3": true}]
```  

下面是输入无效的示例。

Null 值区分大小写，必须全用小写字符（即 "null"）来表示。

```sql
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  

 下面是结果集：  

```json
[{}]
```  

传递的表达式将会解析为 null 表达式；以下输入不会计算为 null 类型，因此会返回未定义。

```sql
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  

 下面是结果集：  

```json
[{}]
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query string to null  -->
<!--New.date: 10/28/2019-->