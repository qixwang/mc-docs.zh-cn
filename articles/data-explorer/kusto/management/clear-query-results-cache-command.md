---
title: 清除查询结果缓存 - Azure 数据资源管理器
description: 本文介绍用于在 Azure 数据资源管理器中清除缓存的数据库架构的管理命令。
services: data-explorer
author: amitof
ms.author: v-tawe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: reference
origin.date: 06/16/2020
ms.date: 07/01/2020
ms.openlocfilehash: a05827009d63b285fd8bcc572d2315e839b33106
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470494"
---
# <a name="clear-query-results-cache"></a>清除查询结果缓存

清除针对上下文数据库所获得的所有[缓存的查询结果](../query/query-results-cache.md)。

**语法**

`.clear` `database` `cache` `queryresults`

**返回**

此命令返回包含以下列的表：

| 列  | 类型     | 说明                                |
| ------- | -------- | ------------------------------------------ |
| NodeId  | `string` | 群集节点的标识符。            |
| 项 | `long`   | 节点清除的项数。 |

**示例**

```kusto
.clear database cache queryresults
```

| NodeId | 项 |
| ------ | ------- |
| Node1  | 42      |
| Node2  | 0       |
