---
title: 命令和查询管理 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的命令和查询管理。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 08/19/2019
ms.date: 07/01/2020
ms.openlocfilehash: d0ae66829078c58c4c721cfda16219e7e91b4317
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470484"
---
# <a name="commands-and-queries-management"></a>命令和查询管理

## <a name="show-commands-and-queries"></a>.show commands-and-queries

`.show` `commands-and-queries` 返回一个表，其中包含已达到最终状态的管理员命令和查询。 这些命令和查询的可用时间为 30 天。

此命令的输出中显示的信息类似于 [.show commands](commands.md) 和 [.show queries](queries.md) 命令，但是它实际上上允许你以简单方式联接两个结果集。

**语法**

`.show` `commands-and-queries`

**输出**

输出架构如下所示：

| ColumnName               | ColumnType |
| ------------------------ | ---------- |
| ClientActivityId         | string     |
| CommandType              | string     |
| 文本                     | string     |
| 数据库                 | string     |
| StartedOn                | datetime   |
| LastUpdatedOn            | datetime   |
| 持续时间                 | timespan   |
| State                    | 字符串     |
| FailureReason            | string     |
| RootActivityId           | GUID       |
| User                     | string     |
| 应用程序              | string     |
| 主体                | string     |
| ClientRequestProperties  | 动态    |
| TotalCpu                 | timespan   |
| MemoryPeak               | long       |
| CacheStatistics          | 动态    |
| ScannedExtentsStatistics | 动态    |
| ResultSetStatistics      | 动态    |

> [!NOTE]
> 对于查询，`CommandType` 的值为 `Query`。
