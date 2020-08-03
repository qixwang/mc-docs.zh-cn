---
title: 查询结果缓存 - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的查询结果缓存。
services: data-explorer
author: amitof
ms.author: v-tawe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: reference
origin.date: 06/16/2020
ms.date: 07/01/2020
ms.openlocfilehash: a38d2c776b8161e6ca91f7dd7ecb585751806207
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470337"
---
# <a name="query-results-cache"></a>查询结果缓存

查询结果缓存是专用于存储查询结果的缓存。 有关详细信息，请参阅[查询结果缓存](../query/query-results-cache.md)。

**查询结果缓存命令**

Kusto 提供了两个用于缓存管理和可观测性的命令：

- [`Show cache`](show-query-results-cache-command.md)：使用此命令显示结果缓存公开的统计信息。

- [`Clear cache(rhs:string)`](clear-query-results-cache-command.md)：使用此命令可清除缓存的结果。
