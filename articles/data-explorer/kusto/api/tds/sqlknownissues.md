---
title: Kusto 与 SQL Server 的 MS-TDS/T-SQL 差异 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中 Kusto 与 Microsoft SQL Server 之间的 MS-TDS/T-SQL 差异。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 09/04/2019
ms.date: 07/01/2020
ms.openlocfilehash: 90cf9352b0e6700ae187bd5d26ed45c6d3e3586e
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226296"
---
# <a name="ms-tdst-sql-differences-between-kusto-microsoft-sql-server"></a>Kusto 与 Microsoft SQL Server 之间的 MS-TDS/T-SQL 差异

下面是 Kusto 与 SQL Server 的 T-SQL 实现之间的主要差异的部分列表。

## <a name="create-insert-drop-alter-statements"></a>CREATE、INSERT、DROP、ALTER 语句

Kusto 不支持通过 MS-TDS 修改架构或数据，也不支持上述 T-SQL 语句。

## <a name="correlated-sub-queries"></a>相关子查询

Kusto 不支持 `SELECT`、`WHERE` 和 `JOIN` 子句中的相关子查询。

## <a name="top-flavors"></a>TOP 风格

Kusto 会忽略 `WITH TIES`，并将查询作为常规 `TOP` 计算。
Kusto 不支持 `PERCENT`。

## <a name="cursors"></a>游标

Kusto 不支持 SQL 游标。

## <a name="flow-control"></a>流量控制

Kusto 不支持流控制语句，但少数情况除外，如 `IF` `THEN` `ELSE` 子句，它与 `THEN` 和 `ELSE` 批处理具有相同的架构。

## <a name="data-types"></a>数据类型

根据查询的不同，返回的数据的类型可能与 SQL Server 中的数据类型不同。
在这里，一个明显的示例是 `TINYINT` 和 `SMALLINT` 之类的类型，它们在 Kusto 中没有等效项。 因此，需要 `BYTE` 或 `INT16` 类型值的客户端可以改为获取 `INT32` 或 `INT64` 值。

## <a name="column-order-in-results"></a>结果中的列顺序

在 `SELECT` 语句中使用星号时，每个结果集中的列顺序在 Kusto 和 SQL Server 之间可能有所不同。 在这些情况下，使用列名的客户端会工作得更好。
如果 `SELECT` 语句中没有星号字符，则会保留列序号。

## <a name="columns-name-in-results"></a>结果中的列名

在 T-SQL 中，多个列可能有相同的名称。 这在 Kusto 中是不允许的。
如果名称发生冲突，则列名在 Kusto 中可能会不同。
但是，至少会保留其中一列的原始名称。

## <a name="any-all-and-exists-predicates"></a>ANY、ALL 和 EXISTS 谓词

Kusto 不支持谓词 `ANY`、`ALL` 和 `EXISTS`。

## <a name="recursive-ctes"></a>递归 CTE

Kusto 不支持递归公用表表达式。

## <a name="dynamic-sql"></a>动态 SQL

Kusto 不支持动态 SQL 语句（以内联方式执行由查询生成的 SQL 脚本）。

## <a name="within-group"></a>WITHIN GROUP

Kusto 不支持 `WITHIN GROUP` 子句。

## <a name="truncate-function"></a>TRUNCATE 函数

Kusto 中 TRUNCATE 函数 (ODBC) 的工作方式类似于 ROUND，这意味着结果将是最接近的值，而不是 SQL 中返回的较小的值。