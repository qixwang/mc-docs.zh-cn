---
title: 缓存策略 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的缓存策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 07/01/2020
ms.openlocfilehash: 2f12b0e5efa1e014204870d49b7934cfdf1c4b6e
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470378"
---
# <a name="cache-policy"></a>缓存策略

本文介绍用于创建和更改[缓存策略](cachepolicy.md)的命令 

## <a name="displaying-the-cache-policy"></a>显示缓存策略

可以在数据或表上设置此策略，并且可以使用以下其中一个命令来显示此策略：

* `.show` `database` DatabaseName `policy` `caching`
* `.show` `table` DatabaseName`.`TableName `policy` `caching` 

## <a name="altering-the-cache-policy"></a>更改缓存策略

```kusto
.alter <entity_type> <database_or_table_name> policy caching hot = <timespan>
```

更改多个表的缓存策略（在相同数据库上下文中）：

```kusto
.alter tables (table_name [, ...]) policy caching hot = <timespan>
```

缓存策略：

```kusto
{
  "DataHotSpan": {
    "Value": "3.00:00:00"
  },
  "IndexHotSpan": {
    "Value": "3.00:00:00"
  }
}
```

* `entity_type`：表、数据库或群集
* `database_or_table`：如果实体为表或数据库，则应通过如下命令指定其名称 
  - `database_name` 或 
  - `database_name.table_name` 或 
  - `table_name`（在特定数据库的上下文中运行时）

## <a name="deleting-the-cache-policy"></a>删除缓存策略

```kusto
.delete <entity_type> <database_or_table_name> policy caching
```

**示例**

显示数据库 `MyDatabase` 中表 `MyTable` 的缓存策略：

```kusto
.show table MyDatabase.MyTable policy caching 
```

将表 `MyTable` 的缓存策略（在数据库上下文中）设置为 3 天：

```kusto
.alter table MyTable policy caching hot = 3d
```

将多个表的策略（在数据库上下文中）设置为 3 天：

```kusto
.alter tables (MyTable1, MyTable2, MyTable3) policy caching hot = 3d
```

删除针对表设置的策略：

```kusto
.delete table MyTable policy caching
```

删除针对数据库设置的策略：

```kusto
.delete database MyDatabase policy caching
```
