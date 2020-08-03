---
title: 流式引入策略 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的流式引入策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/20/2020
ms.date: 07/01/2020
ms.openlocfilehash: 10de6e5e09fead4ba55f979b815a831d145531b2
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470441"
---
# <a name="streaming-ingestion-policy"></a>流式引入策略

## <a name="streaming-ingestion-target-scenario"></a>流式引入目标方案

流式引入适用于需要低延迟且各种量级数据的引入时间小于 10 秒的方案。 它用于优化一个或多个数据库中多个表的操作处理，其中进入每个表的数据流相对较小（每秒几条记录），但总体数据引入量较高（每秒成千上万条记录）。

当每张表的数据量增加到每小时 4 Gb 以上时，请使用经典（批量）引入而不是流式引入。

- 若要了解如何实现此功能，请参阅[流式引入](../../ingest-data-streaming.md)。
- 若要了解流式引入控制命令，请参阅[用于管理流式引入策略的控制命令](streamingingestion-policy.md)

## <a name="streaming-ingestion-policy-definition"></a>流式引入策略定义

流式引入策略包含以下属性：

- **IsEnabled**：
  - 定义表/数据库的流式引入功能的状态
  - 必需，无默认值，必须显式设置为“true”或“false”
- **HintAllocatedRate**：
  - 如果设置，则为表预期的每小时数据量（以 GB 为单位）提供提示。 此提示有助于系统调整分配给表的资源量，以支持流式引入。
  - 默认值“NULL”（取消设置）

若要对表启用流式引入，请定义流式引入策略，将“IsEnabled”设置为“true”。 可以在表本身或数据库上设置此定义。
在数据库级别定义此策略将对数据库中的所有现有表和未来表应用相同的设置。 如果同时在表级别和数据库级别设置流式引入策略，则表级别设置优先。 此设置意味着通常可以为数据库启用流式引入，但可以专门对某些表禁用流式引入，反之亦然。

> [!NOTE]
> 如果表不直接获得流式引入，而只是通过更新策略获得，则不必在此表上定义流式引入策略。

## <a name="set-the-data-rate-hint"></a>设置数据速率提示

流式引入策略可以提供有关表预期的每小时数据量的提示。 此提示有助于系统调整分配给该表的资源量，以支持流式引入。
如果将数据流式引入表的速度超过 1 Gb/小时，则设置此提示。
如果在数据库的流式引入策略中设置“HintAllocatedRate”，则按预期数据速率最高的表设置此项。 建议不要将表的有效提示设置为远高于预期的每小时数据速率峰值的值。 此设置可能会对查询性能产生不利影响。
