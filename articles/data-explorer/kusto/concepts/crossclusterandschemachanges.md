---
title: Kusto 跨群集查询和架构更改 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的跨群集查询和架构更改。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: 91c516977d821f0ac75ed7fc5a61a1475f45ea24
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226289"
---
# <a name="cross-cluster-queries-and-schema-changes"></a>跨群集查询和架构更改

执行跨群集查询时，进行初始查询解释的群集必须具有远程群集中引用的实体的架构。
当以下查询发送到 Cluster1 时

```kusto
Table1 | join ( cluster("Cluster2").database("SomeDB").Table2 ) on KeyColumn
``` 

Cluster1 必须知道 Cluster2 上的 Table2 有哪些列以及这些列的数据类型。 若要获取该信息，需要将特殊命令从 Cluster1 发送到 Cluster2。
发送命令的开销可能很高。 检索到后，将缓存远程实体的架构，并且引用相同实体的下一个查询将运行较少的网络操作。

远程实体架构的更改可能会导致不利影响。 例如，无法识别添加的列，或者删除的列会导致“部分查询错误”而不是语义错误。

为了解决这个问题，缓存的架构会在检索到后一小时过期，因此在更改后超过一小时才执行的任何查询都将使用最新架构。
