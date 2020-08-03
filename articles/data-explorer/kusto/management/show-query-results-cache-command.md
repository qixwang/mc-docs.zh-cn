---
title: 显示查询结果缓存 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .show 查询结果缓存。
services: data-explorer
author: amitof
ms.author: v-tawe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: reference
origin.date: 06/16/2020
ms.date: 07/01/2020
ms.openlocfilehash: e0c9ccf0e1275a7c83c4b7634152879438665008
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470449"
---
# <a name="show-query-results-cache"></a>.show 查询结果缓存

返回一个表，该表显示与[查询结果缓存](../query/query-results-cache.md)有关的统计信息。

**语法**

`.show` `query` `results` `cache`

**输出**

| 输出参数     | 类型 | 说明                                             |
| -------------------- | ---- | ------------------------------------------------------- |
| 命中数                 | long | 缓存命中次数。                               |
| 未命中数               | long | 缓存失误次数。                             |
| CacheCapacityInBytes | long | 缓存容量（以字节为单位）。                            |
| UsedBytes            | long | 缓存使用的空间。                                   |
| 计数                | long | 缓存中存储的唯一查询结果的数量。 |

**限制**

- 该命令的输出当前仅反映请求到达的节点收集的缓存统计信息。
- 该命令仅显示“最近”的历史记录。
