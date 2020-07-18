---
title: T-SQL - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 T-SQL。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: d7a6c2f5141d780d628606ec09cc1774e50ddc9f
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226294"
---
# <a name="t-sql-support"></a>T-SQL 支持

[Kusto 查询语言 (KQL)](../../query/index.md) 是首选查询语言。
但是，T-SQL 支持对于无法轻易转换为使用 KQL 的工具很有用。  
T-SQL 支持也可供熟悉 SQL 的人偶尔使用。

Kusto 可以解释和运行具有一些语言限制的 T-SQL 查询。

> [!NOTE]
> Kusto 不支持 DDL 命令。 仅支持 T-SQL `select` 语句。 若要详细了解与 T-SQL 相关的主要差异，请参阅 [SQL 已知问题](./sqlknownissues.md)。

## <a name="querying-from-kustoexplorer-with-t-sql"></a>使用 T-SQL 从 Kusto.Explorer 查询

Kusto.Explorer 工具支持对 Kusto 进行的 T-SQL 查询。
若要指示 Kusto.Explorer 执行查询，请使用空的 T-SQL 注释行 (`--`) 开始查询。 例如：

```sql
--
select * from StormEvents
```

## <a name="from-t-sql-to-kusto-query-language"></a>从 T-SQL 到 Kusto 查询语言

Kusto 支持将 T-SQL 查询转换为 Kusto 查询语言 (KQL)。 这种转换有助于熟悉 SQL 的人员更好地了解 KQL。
若要从一些 T-SQL `select` 语句返回等效的 KQL，请在查询之前添加 `explain`。

例如，以下 T-SQL 查询：

```sql
--
explain
select top(10) *
from StormEvents
order by DamageProperty desc
```

生成以下输出：

```kusto
StormEvents
| sort by DamageProperty desc nulls first
| take 10
```
