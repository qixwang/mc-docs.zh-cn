---
title: 架构设计的最佳做法 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中架构设计的最佳做法。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/18/2020
ms.date: 07/01/2020
ms.openlocfilehash: a65786d962b3635e78aea4001ffebb59ae0726f5
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470370"
---
# <a name="best-practices-for-schema-design"></a>针对架构设计的最佳做法

可以遵循几个“注意事项”，使你的管理命令更好地执行，并减轻对服务的影响。

## <a name="do"></a>要

1. 如果需要创建多个表，请使用 [`.create tables`](create-tables-command.md) 命令，而不是发出多个 `.create table` 命令。
2. 如果需要重命名多个表，请通过一次调用 [`.rename tables`](rename-table-command.md) 来完成，而不是为每对表分别发出一个单独的调用。
3. 使用局限性最小的 `.show` 命令，而不是在管道 (`|`) 后应用筛选器。 例如：
    - 使用 `.show table T extents` 而不是 `.show cluster extents | where TableName == 'T'`
    - 使用 `.show database DB schema` 而不是 `.show schema | where DatabaseName == 'DB'`。
4. 仅当需要获取单个表的实际统计信息时，才使用 `.show table T`。 如果只需要检查表是否存在，或者只是获取表的架构，请使用 `.show table T schema as json`。
5. 为包含日期/时间值的表定义架构时，请确保键入的这些列的类型为 `datetime`。
    - Kusto 已高度优化，适合对 `datetime` 列进行筛选。 请勿在查询时将 `string` 或数值（例如 `long`）列转换为 `datetime` 进行筛选（如果这可以在引入之前或引入过程中完成）。

## <a name="dont"></a>不要

1. 不要太频繁地运行 `.show` 命令（例如 `.show schema`、`.show databases`、`.show tables`）。 如果可能，请缓存这些命令返回的信息。
2. 请勿在具有大型架构（例如具有 100 多个数据库）的群集上运行 `.show schema` 命令。 请改用 [`.show databases schema`](../management/show-schema-database.md)。
3. 不要太频繁地运行 [command-then-query](index.md#combining-queries-and-control-commands) 操作。
    - command-then-query：表示通过管道传递控制命令的结果集并对其应用筛选器/聚合。
        - 例如： `.show ... | where ... | summarize ...`
    - 运行 `.show cluster extents | count`（强调 `| count`）之类的内容时，Kusto 会先准备一个数据表（其中包含群集中所有盘区的所有详细信息），然后将这个只存在于内存中的表发送到 Kusto 引擎进行计数。 这意味着，实际上，Kusto 为返回给你如此简单的答案，在未优化的路径中进行了很费力的操作。
4. 在数据引入过程中过度使用盘区标记。 在使用 `drop-by:` 标记时尤其如此，这限制了系统在后台执行面向性能的整理过程的能力。
    - 请参阅[此处](../management/extents-overview.md#extent-tagging)的性能说明。
    