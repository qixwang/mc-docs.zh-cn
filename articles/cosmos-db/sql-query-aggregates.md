---
title: Azure Cosmos DB 中的聚合函数
description: 了解 SQL 聚合函数语法以及 Azure Cosmos DB 支持的聚合函数类型。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 12/02/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: c1e1906b911381a1fc55462d5a736bb8ae2d2695
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028714"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Azure Cosmos DB 中的聚合函数

聚合函数对 SELECT 子句中的一组值执行计算，并返回单个值。 例如，以下查询返回 `Families` 容器中的项计数。

## <a name="examples"></a>示例

```sql
    SELECT COUNT(1)
    FROM Families f
```

结果有：

```json
    [{
        "$1": 2
    }]
```

也可以使用 VALUE 关键字来仅返回聚合的标量值。 例如，以下查询将值的计数作为单个值返回：

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

结果有：

```json
    [ 2 ]
```

还可以将聚合与筛选器结合使用。 例如，以下查询返回包含 `WA` 州地址的项计数。

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

结果有：

```json
    [ 1 ]
```

## <a name="types-of-aggregate-functions"></a>聚合函数的类型

SQL API 支持以下聚合函数。 SUM 和 AVG 针对数字值执行运算，COUNT、MIN 和 MAX 可以处理数字、字符串、布尔值和 null。

| 函数 | 说明 |
|-------|-------------|
| COUNT | 在表达式中返回项的数目。 |
| SUM   | 在表达式中返回所有值的总和。 |
| 最小值   | 返回表达式中的最小值。 |
| MAX   | 返回表达式中的最大值。 |
| 平均值   | 在表达式中返回多个值的平均值。 |

还可以基于数组迭代的结果进行聚合。

> [!NOTE]
> 在 Azure 门户的数据资源管理器中，聚合查询可以仅基于一个查询页面聚合部分结果。 SDK 跨所有页面生成单个累计值。 若要使用代码执行聚合查询，需要 .NET SDK 1.12.0、.NET Core SDK 1.1.0，或者 Java SDK 1.9.5 或更高版本。

## <a name="next-steps"></a>后续步骤

- [Azure Cosmos DB 简介](introduction.md)
- [系统函数](sql-query-system-functions.md)
- [用户定义的函数](sql-query-udfs.md)

<!-- Update_Description: wording update, update link -->