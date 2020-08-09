---
title: 表 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的表。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/30/2019
ms.date: 07/31/2020
ms.openlocfilehash: b05cff613fba09b3a9fe1c66ff8a700b8080f4d7
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509130"
---
# <a name="tables"></a>表

表是用于保存数据的命名实体。 表具有一组有序[列](./columns.md)以及零个或零个以上的数据行。 每一行为表的每个列保存一个数据值。 表中的行顺序是未知的，除了某些本身不确定的表格运算符（例如 [top 运算符](../topoperator.md)）外，一般不会影响查询。

表与[存储函数](./stored-functions.md)占用相同的命名空间。
如果存储函数与表同名，则会选择存储函数。

**说明**  

* 表名区分大小写。
* 表名必须遵循[实体名称](./entity-names.md)的规则。
* 每个数据库的表数量上限为 10,000。


有关如何创建和管理表的详细信息，可参阅[管理表](../../management/tables.md)

## <a name="table-references"></a>表引用

引用表的最简单方法是使用其名称。 可对上下文中数据库内的所有表执行此引用。 例如，下面的查询将计算当前数据库的 `StormEvents` 表的记录数：

```kusto
StormEvents
| count
```

编写上述查询的等效方法是转义表名：

```kusto
["StormEvents"]
| count
```

还可以通过显式注明表所在的数据库（或数据库和群集）来引用表。 然后，可以创建合并多个数据库和群集中的数据的查询。 例如，只要调用方有权访问目标数据库，下面的查询就可用于上下文中的任何数据库：

```kusto
cluster("https://help.kusto.chinacloudapi.cn").database("Samples").StormEvents
| count
```

也可以通过使用 [table() 特殊函数](../tablefunction.md)来引用表，只要该函数的参数计算结果是一个常量。 例如：

```kusto
let counter=(TableName:string) { table(TableName) | count };
counter("StormEvents")
```

> [!NOTE]
> 使用 `table()` 特殊函数显式指定表数据范围。 例如，使用此函数可将处理限制为位于热缓存的表中的数据。
