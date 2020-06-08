---
title: 使用 Azure Monitor 监视 Azure 资源 | Microsoft Docs
description: 介绍如何使用 Azure Monitor 从 Azure 的资源中收集和分析监视数据。
ms.subservice: logs
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 05/22/2020
ms.openlocfilehash: f4ffb32ee7341660e84b0858973ce8f4f9a64bec
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200151"
---
# <a name="monitoring-azure-resources-with-azure-monitor"></a>使用 Azure Monitor 监视 Azure 资源
当你的关键应用程序和业务流程依赖于 Azure 资源时，你需要监视这些资源的可用性、性能和操作。 本文介绍 Azure 资源生成的监视数据，以及如何使用 Azure Monitor 的功能在此数据的基础上进行分析和发出警报。

> [!IMPORTANT]
> 本文适用于 Azure 中使用 Azure Monitor 的所有服务。 计算资源（包括 VM 和应用服务）生成与此处所述相同的监视数据，但还有来宾操作系统也可能生成日志和指标。 有关如何收集和分析此数据的详细信息，请参阅这些服务的监视文档。

## <a name="what-is-azure-monitor"></a>说明是 Azure Monitor？
Azure Monitor 是 Azure 中的一个全堆栈监视服务，提供用于监视 Azure 资源以及其他云中资源和本地资源的整套功能。 [Azure Monitor 数据平台](../platform/data-platform.md)将数据收集到[日志](../platform/data-platform-logs.md)和[指标](../platform/data-platform-metrics.md)中，在这两个位置可以使用一组完整的监视工具对数据进行分析，如以下各节所述。

- [可对 Azure Monitor 指标执行哪些操作？](../platform/data-platform-metrics.md#what-can-you-do-with-azure-monitor-metrics)
- [可对 Azure Monitor 日志执行哪些操作？](../platform/data-platform-logs.md#what-can-you-do-with-azure-monitor-logs)

创建 Azure 资源后，就会启用 Azure Monitor，并开始收集指标和活动日志，可以在 [Azure 门户中查看和分析](#monitoring-in-the-azure-portal)这些指标和日志。 通过一些配置，可以收集其他监视数据并启用其他功能。 有关配置要求的详细信息，请参阅下面的[监视数据](#monitoring-data)。


## <a name="costs-associated-with-monitoring"></a>与监视相关的成本
分析默认收集的监视数据不收取任何费用。 其中包括：

- 收集平台指标，并通过指标资源管理器对其进行分析。
- 收集活动日志，并在 Azure 门户中对其进行分析。
- 创建活动日志警报规则。

收集和导出日志和指标不会产生 Azure Monitor 成本，但可能存在与目标位置相关的成本：

- 在 Log Analytics 工作区中收集日志和指标时，与数据引入和保留相关的成本。 请参阅 [Log Analytics 的 Azure Monitor 定价](https://www.azure.cn/pricing/details/monitor/)。
- 将日志和指标收集到 Azure 存储帐户时，与数据存储相关的成本。 请参阅 [Blob 存储的 Azure 存储定价](https://www.azure.cn/pricing/details/storage/blobs/)。
- 将日志和指标转发到 Azure 事件中心时，与事件中心流式处理相关的成本。 请参阅 [Azure 事件中心定价](https://www.azure.cn/pricing/details/event-hubs/)。

可能存在与以下操作相关的 Azure Monitor 成本。 请参阅 [Azure Monitor 定价](https://www.azure.cn/pricing/details/monitor/)：

- 运行日志查询。
- 创建指标或日志查询警报规则。
- 从任何警报规则发送通知。
- 通过 API 访问指标。

## <a name="monitoring-data"></a>监视数据
Azure 中的资源生成[日志](../platform/data-platform-logs.md)和[指标](../platform/data-platform-metrics.md)，如下图所示。 有关它们生成的特定数据以及它们提供的任何其他解决方案或见解，请参阅每个 Azure 服务的文档。

![概述](./media/monitor-azure-resource/logs-metrics.png)



- [平台指标](../platform/data-platform-metrics.md) - 定期自动收集的数值，用于描述资源在某一特定时间的某些情况。 
- [资源日志](../platform/platform-logs-overview.md) - 深入了解在 Azure 资源（数据平面）内执行的操作，例如，从 Key Vault 获取机密，或向数据库发出请求。 资源日志的内容和结构因 Azure 服务和资源类型而异。
- [活动日志](../platform/platform-logs-overview.md) - 深入了解从外部（管理平台）对订阅中的每个 Azure 资源执行的操作，例如创建新资源或启动虚拟机。 这是有关订阅中资源上任何写入操作（PUT、POST、DELETE）的操作内容、操作者和操作时间等信息。


## <a name="configuration-requirements"></a>配置要求

### <a name="configure-monitoring"></a>配置监视
一些监视数据是自动收集的，但可能需要根据需求执行一些配置。 有关每种类型的监视数据的特定信息，请参阅下面的信息。

- [平台指标](../platform/data-platform-metrics.md) - 平台指标自动收集到 [Azure Monitor 指标](../platform/data-platform-metrics.md)中，无需进行任何配置。 创建诊断设置以将条目发送到 Azure Monitor 日志，或将其转发到 Azure 之外。
- [资源日志](../platform/platform-logs-overview.md) - 资源日志由 Azure 资源自动生成，但在没有诊断设置的情况下不会收集。  创建诊断设置以将条目发送到 Azure Monitor 日志，或将其转发到 Azure 之外。
- [活动日志](../platform/platform-logs-overview.md) - 活动日志是自动收集的，无需任何配置，可在 Azure 门户中查看。 创建诊断设置以将它们复制到 Azure Monitor 日志，或将其转发到 Azure 之外。

### <a name="log-analytics-workspace"></a>Log Analytics 工作区
将数据收集到 Azure Monitor 日志中需要 Log Analytics 工作区。 可以通过创建新的工作区来快速开始监视服务，但是使用从其他服务收集数据的工作区可能会有所帮助。 有关创建工作区的详细信息，请参阅[在 Azure 门户中创建 Log Analytics 工作区](../learn/quick-create-workspace.md)和[设计 Azure Monitor 日志部署](../platform/design-logs-deployment.md)以帮助确定适合你需求的最佳工作区设计。 如果使用组织中的现有工作区，则需要适当的权限，如[在 Azure Monitor 中管理对日志数据和工作区的访问](../platform/manage-access.md)中所述。 





## <a name="diagnostic-settings"></a>诊断设置
诊断设置定义特定资源的资源日志和指标所发送到的位置。 可能的目标为：

- 通过[Log Analytics 工作区](../platform/resource-logs-collect-workspace.md)可使用强大的日志查询结合 Azure Monitor 收集的其他监视数据对数据进行分析，并利用其他 Azure Monitor 功能，例如警报和可视化。 
- 使用[事件中心](../platform/resource-logs-stream-event-hubs.md)可将数据流式传输到外部系统，例如第三方 SIEM 和其他日志分析解决方案。 
- [Azure 存储帐户](../platform/resource-logs-collect-storage.md)有助于审核、静态分析或备份。

请按照[创建诊断设置以收集 Azure 中的平台日志和指标](../platform/diagnostic-settings.md)中的步骤操作，以便通过 Azure 门户创建和管理诊断设置。 请参阅[使用资源管理器模板在 Azure 中创建诊断设置](../platform/diagnostic-settings-template.md)以在模板中定义这些设置并在创建资源时对其启用完全监视。


## <a name="monitoring-in-the-azure-portal"></a>在 Azure 门户中进行监视
 可以从 Azure 门户中的资源菜单访问大多数 Azure 资源的监视数据。 这样，你就可以使用标准 Azure Monitor 工具访问单项资源的数据。 某些 Azure 服务将提供不同的选项，因此应参考该服务的文档以获取其他信息。 使用 Azure Monitor 菜单分析来自所有受监视资源的数据。 

### <a name="overview"></a>概述
许多服务的“概述”页中会包含监视数据，以提供关于操作的快速概览。 这通常会基于 Azure Monitor 指标中存储的平台指标子集。 服务的“监视”部分通常会提供其他监视选项。 菜单。

![概述页](./media/monitor-azure-resource/overview-page.png)


### <a name="insights-and-solutions"></a>见解和解决方案 
某些服务将提供 Azure Monitor 标准功能以外的工具。 [见解](../insights/insights-overview.md)提供了基于 Azure Monitor 数据平台和标准功能的自定义监视体验。 [解决方案](../insights/solutions.md)提供了基于 Azure Monitor 日志的预定义监视逻辑。 

如果服务具有 Azure Monitor 见解，可以从每项资源菜单中的“监视”对其进行访问。 从 Azure Monitor 菜单访问所有见解和解决方案。

![见解](./media/monitor-azure-resource/insights.png)

### <a name="metrics"></a>指标
使用[指标资源管理器](../platform/metrics-getting-started.md)（在大多数服务的“指标”菜单项中提供）来分析 Azure 门户中的指标。 借助此工具可使用单个指标或将多个指标组合在一起，以确定关联与趋势。 

- 有关使用指标资源管理器的基础知识，请参阅 [Azure 指标资源管理器入门](../platform/metrics-getting-started.md)。
- 请参阅 [Azure 指标资源管理器的高级功能](../platform/metrics-charts.md)，以了解指标资源管理器的高级功能，例如使用多个指标、应用筛选器和拆分。

![指标](./media/monitor-azure-resource/metrics.png)


### <a name="activity-log"></a>活动日志 
查看 Azure 门户的活动日志中的条目，将初始筛选器设置为当前资源。 将活动日志复制到 Log Analytics 工作区以访问该日志，从而在日志查询和工作簿中使用它。 

- 有关查看活动日志和使用各种方法检索条目的详细信息，请参阅[查看和检索 Azure 活动日志事件](../platform/activity-log-view.md)。
- 请参阅 Azure 服务的文档以获取记录的特定事件。

![活动日志](./media/monitor-azure-resource/activity-log.png)

### <a name="azure-monitor-logs"></a>Azure Monitor 日志
Azure Monitor 日志合并了来自多个服务和其他数据源的日志和指标，以使用功能强大的查询工具进行分析。 如上所述，创建诊断设置以将平台指标、活动日志和资源日志收集到 Azure Monitor 的 Log Analytics 工作区中。

通过 [Log Analytics](../log-query/get-started-portal.md) 可使用[日志查询](../log-query/log-query-overview.md)，这是 Azure Monitor 的一项强大功能，允许你使用一种功能齐全的查询语言对日志数据执行高级分析。 从“监视”菜单中的“日志”中打开 Log Analytics，以使 Azure资源可以响应使用该资源作为[查询范围](../log-query/scope.md#query-scope)的日志查询 。 这允许你仅针对该资源分析来自多个表的数据。 使用 Azure Monitor 菜单中的“日志”访问所有资源的日志。 

- 有关使用用于编写日志查询的查询语言的教程，请参阅 [Azure Monitor 中的日志查询入门](../log-query/get-started-queries.md)。
- 有关如何在 Azure Monitor 日志中收集资源日志的信息以及如何在查询中访问资源日志的详细信息，请参阅[在 Azure Monitor 的 Log Analytics 工作区中收集 Azure 资源日志](../platform/resource-logs-collect-workspace.md)。
- 有关 Azure Monitor 日志中的资源日志数据构建方式的说明，请参阅[收集模式](../platform/resource-logs-collect-workspace.md#resource-log-collection-mode)。
- 请参阅每个 Azure 服务的文档，以了解其在 Azure Monitor 日志中的表的详细信息。

![日志](./media/monitor-azure-resource/logs.png)

## <a name="monitoring-from-command-line"></a>从命令行进行监视
可以使用 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/) 或 [Azure 命令行界面](/cli/)从命令行访问从资源收集的监视数据，或将其包含在脚本中。 

- 有关从 CLI 访问指标数据的信息，请参阅 [CLI 指标参考](/cli/monitor/metrics)。
- 有关从 CLI 使用日志查询访问 Azure Monitor 日志数据的信息，请参阅 [CLI Log Analytics 参考](https://docs.microsoft.com/cli/ext/log-analytics/monitor/log-analytics)。
- 有关从 Azure PowerShell 访问指标数据的信息，请参阅 [Azure PowerShell 指标参考](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric)。
- 有关使用 Azure PowerShell 中的日志查询访问 Azure Monitor 日志数据的信息，请参阅 [Azure PowerShell 日志查询参考](https://docs.microsoft.com/powershell/module/az.operationalinsights/Invoke-AzOperationalInsightsQuery)。

## <a name="monitoring-from-rest-api"></a>从 REST API 进行监视
包括使用 REST API 从自定义应用程序中的资源收集的监视数据。

- 有关从 Azure Monitor REST API 访问指标的详细信息，请参阅 [Azure 监视 REST API 演练](../platform/rest-api-walkthrough.md)。
- 有关使用 Azure PowerShell 中的日志查询访问 Azure Monitor 日志数据的信息，请参阅 [CLI Log Analytics 参考](https://dev.loganalytics.io/)。

## <a name="alerts"></a>警报
在监视数据中发现重要情况时，[警报](../platform/alerts-overview.md)会主动通知你并可能采取行动。 你将创建一条警报规则，定义警报的目标、是否创建警报的条件以及为响应而采取的任何操作。

不同类型的监视数据用于不同类型的警报规则。

- [活动日志警报](../platform/alerts-activity-log.md) - 在活动日志中创建与特定条件匹配的条目时，将创建警报。 例如，可进行配置，以便在创建特定类型的资源后或配置更改失败时收到通知。
- [指标警报](../platform/alerts-metric.md) - 在指标超过特定阈值时创建警报。 指标警报的响应能力比其他警报更强，并且可在问题得到纠正时自动消除。
- [日志查询警报](../platform/alerts-log.md) - 定期运行日志查询，并在发现特定条件时创建警报。 使用该警报可以跨执行多组数据的复杂分析。

使用资源菜单中的“警报”来查看警报并管理该资源的警报规则。 仅活动日志警报和指标警报使用单项 Azure 资源作为目标。 日志查询警报将 Log Analytics 工作区用作目标，并基于可访问存储在该工作区中的任何日志的查询。 使用 Azure Monitor 菜单查看和管理所有资源的警报并管理日志查询警报规则。

- 有关创建警报规则的详细信息，请参阅上述有关各类警报的文章。
- 有关创建用于管理警报响应的操作组的详细信息，请参阅[在 Azure 门户中创建和管理器操作组](../platform/action-groups.md)。



## <a name="next-steps"></a>后续步骤

* 有关不同 Azure 服务的资源日志的详细信息，请参阅 [Azure 资源日志支持的服务、架构和类别](../platform/diagnostic-logs-schema.md)。  

