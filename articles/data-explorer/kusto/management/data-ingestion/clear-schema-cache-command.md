---
title: 清除用于流式引入的缓存架构 - Azure 数据资源管理器
description: 本文介绍用于在 Azure 数据资源管理器中清除缓存的数据库架构的管理命令。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 05/20/2020
ms.date: 07/01/2020
ms.openlocfilehash: cfa3bbe9249adea16ee16c979323c24d006c8787
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226200"
---
# <a name="clear-schema-cache-for-streaming-ingestion"></a>清除用于流式引入的架构缓存

群集节点会缓存通过流式引入接收数据的数据库的架构。 此过程会优化群集资源的性能和利用率，但可能会在架构更改时导致传播延迟。
清除缓存可确保后续流式引入请求包含数据库或表的架构更改。
有关详细信息，请参阅[流式引入和架构更改](streaming-ingestion-schema-changes.md)。

**清除架构缓存**

`.clear cache streamingingestion schema` 命令从所有群集节点刷新缓存的架构。

**语法**

`.clear` `table` &lt;表名称&gt; `cache` `streamingingestion` `schema`

`.clear` `database` `cache` `streamingingestion` `schema`

**返回**

此命令返回包含以下列的表：

|列    |类型    |说明
|---|---|---
|NodeId|`string`|群集节点的标识符
|状态|`string`|成功/失败

**示例**

```kusto
.clear database cache streamingingestion schema

.show table T1 cache streamingingestion schema
```

|NodeId|状态|
|---|---|
|Node1|已成功
|Node2|已失败

> [!NOTE]
> 如果命令失败或返回的表中的某一行包含“Status=Failed”，则可以安全地重试该命令。
