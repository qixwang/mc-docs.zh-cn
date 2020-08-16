---
title: database()（范围函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 database()（范围函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
zone_pivot_group_filename: data-explorer/zone-pivot-groups.json
zone_pivot_groups: kql-flavors
ms.openlocfilehash: 102806cfa7ba74714500984c7c7af6dcb14be280
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841660"
---
# <a name="database-scope-function"></a>database()（范围函数）

::: zone pivot="azuredataexplorer"

将查询的引用更改为群集范围内的特定数据库。 

```kusto
database('Sample').StormEvents
cluster('help').database('Sample').StormEvents
```

**语法**

`database(`*stringConstant*`)`

**参数**

* *stringConstant*：被引用数据库的名称。 标识的数据库可以是 `DatabaseName` 或 `PrettyName`。 在执行查询之前，参数必须是常量，即不能来自子查询求值。

**备注**

* 若要访问远程群集和远程数据库，请参阅 [cluster()](clusterfunction.md) 范围函数。
* 有关跨群集查询和跨数据库查询的详细信息，请参阅[此文](cross-cluster-or-database-queries.md)

## <a name="examples"></a>示例

### <a name="use-database-to-access-table-of-other-database"></a>使用 database() 访问其他数据库的表。 

```kusto
database('Samples').StormEvents | count
```

|计数|
|---|
|59066|

### <a name="use-database-inside-let-statements"></a>在 let 语句中使用 database() 

可以重写与上述查询相同的查询，以便使用可接收参数 `dbName` 的内联函数（let 语句）- 该参数会传递到 database() 函数中。

```kusto
let foo = (dbName:string)
{
    database(dbName).StormEvents | count
};
foo('help')
```

|计数|
|---|
|59066|

### <a name="use-database-inside-functions"></a>在 Functions 内使用 database() 

可以重写与上述查询相同的查询，以便在可接收参数 `dbName` 的函数中使用它 - 该参数会传递到 database() 函数中。

```kusto
.create function foo(dbName:string)
{
    database(dbName).StormEvents | count
};
```

**请注意：** 此类函数只能在本地使用，而不能在跨群集查询中使用。

::: zone-end

::: zone pivot="azuremonitor"

Azure Monitor 不支持此功能

::: zone-end
