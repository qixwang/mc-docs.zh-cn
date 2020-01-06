---
title: 可管理性和监视 - 查询活动、资源利用率
description: 了解可以使用哪些功能来管理和监视 Azure SQL 数据仓库。 使用 Azure 门户和动态管理视图 (DMV) 来了解数据仓库的查询活动和资源利用率。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
origin.date: 08/09/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 5dbdec38ca321c55b1805864258d1f7f8e3c0085
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75623630"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-sql-data-warehouse"></a>监视 Azure SQL 数据仓库中的资源利用率和查询活动
Azure SQL 数据仓库在 Azure 门户中提供丰富的监视体验用于洞察数据仓库工作负荷。 建议使用 Azure 门户来监视数据仓库，因为它提供可配置的保持期、警报、建议，并为指标和日志提供可自定义的图表与仪表板。 在门户中，还可与 Operations Management Suite (OMS)、Azure Monitor（日志）等其他 Azure 监视服务集成。这样，不仅可以针对数据仓库，而且还能针对整个 Azure 分析平台提供一体式监视体验，构成一种集成式监视体验。 本文档介绍可在 SQL 数据仓库中使用哪些监视功能来优化和管理分析平台。 

## <a name="resource-utilization"></a>资源利用率 
Azure 门户中提供了以下可用于 SQL 数据仓库的指标。 这些指标通过 [Azure Monitor](/azure-monitor/platform/data-collection#metrics) 显示。


| 指标名称             | 说明                                                  | 聚合类型 |
| ----------------------- | ------------------------------------------------------------ | ---------------- |
| CPU 百分比          | 数据仓库所有节点的 CPU 利用率      | 最大值          |
| 数据 IO 百分比      | 数据仓库所有节点的 IO 利用率       | 最大值          |
| 内存百分比       | 数据仓库所有节点的内存利用率 (SQL Server) | 最大值          |
| 成功的连接数  | 数据的成功连接数                 | 总计            |
| 失败的连接数      | 数据仓库的失败连接数           | 总计            |
| 被防火墙阻止     | 登录数据仓库受阻次数     | 总计            |
| DWU 限制               | 数据仓库的服务级别目标                | 最大值          |
| DWU 百分比          | CPU 百分比与数据 IO 百分比之间的最大值        | 最大值          |
| 已用的 DWU                | DWU 限制 * DWU 百分比                                   | 最大值          |
| 缓存命中百分比    | (缓存命中数/缓存未命中数) * 100，其中，缓存命中数是在本地 SSD 缓存中所有列存储段的总命中数，缓存未命中数是所有节点上本地 SSD 缓存中列存储段的未命中数之和 | 最大值          |
| 缓存使用百分比   | (已用缓存/缓存容量) * 100，其中，已用缓存是所有节点上的本地 SSD 缓存中所有字节之和，缓存容量是所有节点上的本地 SSD 缓存存储容量之和 | 最大值          |
| 本地 tempdb 百分比 | 所有计算节点上的本地 tempdb 利用率 - 每五分钟发出一次值 | 最大值          |

> 查看指标和设置警报时要考虑的事项：
>
> - 为特定数据仓库（而不是逻辑服务器）报告失败和成功的连接
> - 即使数据仓库处于空闲状态，内存百分比也会反映利用率，而不会反映活动工作负荷内存消耗。 使用并跟踪这个指标以及其他指标（tempdb、gen2 缓存），以全面判断扩展额外的缓存容量是否会提高工作负荷性能以满足你的需求。


## <a name="query-activity"></a>查询活动
为了让用户通过 T-SQL 以编程方式监视 SQL 数据仓库，该服务提供了一系列动态管理视图 (DMV)。 在主动排查和识别工作负荷的性能瓶颈时，这些视图非常有用。

若要查看 SQL 数据仓库提供的 DMV 列表，请参阅此[文档](/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs)。 

## <a name="metrics-and-diagnostics-logging"></a>指标和诊断日志记录
指标和日志都可导出到 Azure Monitor（具体而言，是 [Azure Monitor 日志](/azure-monitor/log-query/log-query-overview)组件），并可通过[日志查询](/azure-monitor/log-query/get-started-portal)以编程方式访问它们。 SQL 数据仓库的日志延迟大约为 10-15 分钟。 有关影响延迟的因素的更多详细信息，请访问以下文档。


## <a name="next-steps"></a>后续步骤
以下操作方法指南介绍了在监视和管理数据仓库时可以参考的常见方案和用例：

- [使用 DMV 监视数据仓库工作负荷](/sql-data-warehouse/sql-data-warehouse-manage-monitor)
