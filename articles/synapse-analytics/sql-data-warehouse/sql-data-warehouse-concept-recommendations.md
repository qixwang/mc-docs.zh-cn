---
title: Synapse SQL 建议
description: 了解 Synapse SQL 建议及其生成方式
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 02/05/2020
ms.date: 05/11/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 94d870cb459603b05867d5aa351d8ff8fc479a75
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198746"
---
# <a name="synapse-sql-recommendations"></a>Synapse SQL 建议

本文介绍了通过 Azure 顾问提供的 Synapse SQL 建议。  

SQL Analytics 提供了一些建议，确保数据仓库工作负载始终针对性能进行优化。 建议与 [Azure 顾问](../../advisor/advisor-performance-recommendations.md?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)紧密集成，直接在 [Azure 门户](https://portal.azure.cn/#blade/Microsoft_Azure_Expert/AdvisorBlade)中为你提供最佳做法。 SQL Analytics 每天收集活动工作负载的遥测数据并提出建议。 下面概述了支持的建议方案，以及如何应用建议的操作。

可以立即[查看你的建议](https://portal.azure.cn/#blade/Microsoft_Azure_Expert/AdvisorMenuBlade)！ 目前，此功能仅适用于第 2 代数据仓库。 

## <a name="data-skew"></a>数据倾斜

运行工作负荷时，数据倾斜可能会造成额外的数据移动或资源瓶颈。 以下文档介绍了如何识别数据倾斜，以及如何通过选择最佳的分布键来防止发生数据倾斜。

- [识别和消除倾斜](sql-data-warehouse-tables-distribute.md#how-to-tell-if-your-distribution-column-is-a-good-choice)

## <a name="no-or-outdated-statistics"></a>无统计信息或统计信息过时

统计信息欠佳可能严重影响查询性能，因为它可能导致 SQL 查询优化器生成欠佳的查询计划。 以下文档介绍了有关创建和更新统计信息的最佳做法：

- [创建和更新表统计信息](sql-data-warehouse-tables-statistics.md)

若要查看受这些建议影响的表列表，请运行以下 [T-SQL 脚本](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables)。 顾问会不断运行相同的 T-SQL 脚本以生成这些建议。

## <a name="replicate-tables"></a>复制表

为了提供复制表的建议，顾问将基于以下物理特征检测表候选项：

- 复制表的大小
- 列数
- 表分布类型
- 分区数

顾问持续利用基于工作负荷的试探法（例如，表的访问频率、平均返回的行数、有关数据仓库大小和活动的阈值）来确保生成优质建议。

下面介绍了可在 Azure 门户中看到的，用于提供每条复制表建议的基于工作负荷的试探法：

- 扫描平均值 - 在过去七天每次访问表时，从表中返回的行的平均百分比
- 频繁读取且不更新 - 表示该表在过去七天尚未更新，同时显示访问活动
- 读取/更新频率 - 访问表的频率，相对于过去七天更新表的时间
- 活动 - 基于访问活动度量使用情况。 此值将与相对于过去七天内数据仓库中的平均表访问活动的表访问活动进行比较。

目前，顾问一次性只会显示包含聚集列存储索引的、具有最高活动优先级的最多四个复制表候选项。

> [!IMPORTANT]
> 复制表建议不是完整的证据，且未考虑帐户数据移动操作。 我们正在努力将此添加为一种试探方法，但在此期间，请始终在应用建议后验证工作负荷。 如果你发现复制表建议导致工作负荷退化，请联系 sqldwadvisor@service.microsoft.com。 若要详细了解复制表，请访问以下[文档](design-guidance-for-replicated-tables.md#what-is-a-replicated-table)。
