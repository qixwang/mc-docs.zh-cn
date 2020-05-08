---
title: 可管理性和监视 - 查询活动、资源利用率
description: 了解可以使用哪些功能来管理和监视 Azure Synapse Analytics。 使用 Azure 门户和动态管理视图 (DMV) 来了解数据仓库的查询活动和资源利用率。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 04/09/2020
ms.date: 05/11/2020
ms.author: v-jay
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 86750138d58730393de3e8b5f89b7a9064deeefb
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198518"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-synapse-analytics"></a>监视 Azure Synapse Analytics 中的资源利用率和查询活动

Azure Synapse Analytics 在 Azure 门户中提供了丰富的监视体验，可以显示与数据仓库工作负载相关的见解。 建议使用 Azure 门户来监视数据仓库，因为它提供可配置的保持期、警报、建议，并为指标和日志提供可自定义的图表与仪表板。 在门户中，还可集成其他 Azure 监视服务（例如 Azure Monitor（日志））与 Log Analytics，这样，不仅可以针对数据仓库，而且还能针对整个 Azure 分析平台提供一体式监视体验，构成一种集成式监视体验。 本文档介绍可在 SQL Analytics 中使用哪些监视功能来优化和管理分析平台。

## <a name="resource-utilization"></a>资源利用率

Azure 门户中提供了以下可用于 SQL Analytics 的指标。 这些指标通过 [Azure Monitor](../../azure-monitor/platform/data-collection.md?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#metrics) 显示。

| 指标名称             | 说明                                                  | 聚合类型 |
| ----------------------- | ------------------------------------------------------------ | ---------------- |
| CPU 百分比          | 数据仓库所有节点的 CPU 利用率      | Avg、Min、Max    |
| 数据 IO 百分比      | 数据仓库所有节点的 IO 利用率       | Avg、Min、Max    |
| 内存百分比       | 数据仓库所有节点的内存利用率 (SQL Server) | Avg、Min、Max   |
| 活动查询数          | 正在系统上执行的活动查询数             | Sum              |
| 排队的查询数          | 等待开始执行的排队查询数          | Sum              |
| 成功的连接数  | 针对数据库的成功连接数（登录数） | Sum、Count       |
| 失败的连接数      | 针对数据库的失败连接数（登录数） | Sum、Count       |
| 被防火墙阻止     | 登录数据仓库受阻次数     | Sum、Count       |
| DWU 限制               | 数据仓库的服务级别目标                | Avg、Min、Max    |
| DWU 百分比          | CPU 百分比与数据 IO 百分比之间的最大值        | Avg、Min、Max    |
| 已用的 DWU                | DWU 限制 * DWU 百分比                                   | Avg、Min、Max    |
| 缓存命中百分比    | (缓存命中数/缓存未命中数) * 100，其中，缓存命中数是在本地 SSD 缓存中所有列存储段的总命中数，缓存未命中数是所有节点上本地 SSD 缓存中列存储段的未命中数之和 | Avg、Min、Max    |
| 缓存使用百分比   | (已用缓存/缓存容量) * 100，其中，已用缓存是所有节点上的本地 SSD 缓存中所有字节之和，缓存容量是所有节点上的本地 SSD 缓存存储容量之和 | Avg、Min、Max    |
| 本地 tempdb 百分比 | 所有计算节点上的本地 tempdb 利用率 - 每五分钟发出一次值 | Avg、Min、Max    |
| 数据存储大小(GB) | 数据库总计大小。 这包括已使用的、保留的和未分配的空间。 将为数据库保留未分配的空间，以便优化查询和加载性能。 | Sum |
| 灾难恢复大小(GB) | 每 24 小时创建的异地备份的总大小 | Sum |
| 快照存储大小(GB) | 所创建的用于提供数据库还原点的快照的总大小。 这包括自动的和用户定义的快照。 | Sum |

查看指标和设置警报时要考虑的事项：

- 使用的 DWU 仅大概表示 SQL 池中的使用情况，不是一个综合的利用率指标。  若要确定是要纵向扩展还是纵向缩减，请考虑可能会受 DWU 影响的所有因素，例如并发性、内存、tempdb 和自适应缓存容量。 建议[以不同的 DWU 设置运行工作负载](sql-data-warehouse-manage-compute-overview.md#finding-the-right-size-of-data-warehouse-units)，以确定哪种设置最适合业务目标。
- 为特定数据仓库（而不是逻辑服务器）报告失败和成功的连接
- 即使数据仓库处于空闲状态，内存百分比也会反映利用率，而不会反映活动工作负荷内存消耗。 使用并跟踪这个指标以及其他指标（tempdb、gen2 缓存），以全面判断扩展额外的缓存容量是否会提高工作负荷性能以满足你的需求。

## <a name="query-activity"></a>查询活动

为了让用户通过 T-SQL 以编程方式监视 SQL Analytics，该服务提供了一系列动态管理视图 (DMV)。 在主动排查和识别工作负荷的性能瓶颈时，这些视图非常有用。

若要查看适用于 Synapse SQL 的 DMV 的列表，请参阅此[文档](./sql-data-warehouse-reference-tsql-system-views.md#sql-data-warehouse-dynamic-management-views-dmvs)。 

## <a name="metrics-and-diagnostics-logging"></a>指标和诊断日志记录 

指标和日志都可导出到 Azure Monitor（具体而言，是 [Azure Monitor 日志](../../azure-monitor/log-query/log-query-overview.md?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)组件），并可通过[日志查询](../../azure-monitor/log-query/get-started-portal.md?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)以编程方式访问它们。 SQL Analytics 的日志延迟大约为 10-15 分钟。 有关影响延迟的因素的更多详细信息，请访问以下文档。

## <a name="next-steps"></a>后续步骤

以下操作方法指南介绍了在监视和管理数据仓库时可以参考的常见方案和用例：

- [使用 DMV 监视数据仓库工作负荷](sql-data-warehouse-manage-monitor.md)
