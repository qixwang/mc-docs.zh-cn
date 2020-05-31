---
title: Azure Monitor 工作簿数据源 | Microsoft Docs
description: 使用从多个数据源预生成的以及自定义的参数化 Azure Monitor 工作簿简化复杂的报表
services: azure-monitor
documentationcenter: ''
author: Johnnytechn
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: v-johya
ms.openlocfilehash: 682e49852966ad8c411e80ee2a569d1bf49e4196
ms.sourcegitcommit: a04b0b1009b0c62f2deb7c7acee75a1304d98f87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83797038"
---
# <a name="azure-monitor-workbooks-data-sources"></a>Azure Monitor 工作簿数据源

工作簿与大量的数据源兼容。 本文详细介绍当前可用于 Azure Monitor 工作簿的数据源。

## <a name="logs"></a>日志

工作簿允许查询来自以下源的日志：

* Azure Monitor 日志（Application Insights 资源和 Log Analytics 工作区。）
* 以资源为中心的数据（活动日志）

工作簿作者可以使用用于转换基础资源数据的 KQL 查询，以选择可作为文本、图表或网格可视化的结果集。

![工作簿日志报表界面的屏幕截图](./media/workbooks-overview/logs.png)

工作簿作者可以轻松查询多个资源，以创建真正统一的丰富报表体验。

## <a name="metrics"></a>指标

Azure 资源发出可以通过工作簿访问的[指标](data-platform-metrics.md)。 可以在工作簿中通过一个专用控件访问指标，该控件可让你指定目标资源、所需指标和聚合。 然后，可以在图表或网格中绘制这些数据。

![CPU 利用率工作簿指标图表的屏幕截图](./media/workbooks-overview/metrics-graph.png)

![工作簿指标界面的屏幕截图](./media/workbooks-overview/metrics.png)

## <a name="azure-resource-graph"></a>Azure Resource Graph 

工作簿支持使用 Azure Resource Graph (ARG) 查询资源及其元数据。 此功能主要用于生成报表的自定义查询范围。 资源范围通过 ARG 支持的 KQL 子集来表示 - 这通常足以满足常见用例。

若要让查询控件使用此数据源，请使用“查询类型”下拉列表选择“Azure Resource Graph”，然后选择目标订阅。 使用“查询”控件添加选择有意义资源子集的 ARG KQL 子集。


![Azure Resource Graph KQL 查询的屏幕截图](./media/workbooks-overview/azure-resource-graph.png)

## <a name="alerts-preview"></a>警报（预览版）

工作簿允许用户可视化与其资源相关的活动警报。 使用此功能可以创建报表，以便将通知数据（警报）和诊断信息（指标、日志）汇集到一份报表中。 还可以将这些信息联接到一起来创建丰富的报表，以合并通过这些数据源获得的见解。

若要让查询控件使用此数据源，请使用“查询类型”下拉列表选择“警报”，然后选择目标订阅、资源组或资源。 使用警报筛选器下拉列表根据分析需求选择一个有意义的警报子集。

![警报查询的屏幕截图](./media/workbooks-overview/alerts.png)

## <a name="workload-health-preview"></a>工作负荷运行状况（预览版）

Azure Monitor 提供主动监视 Windows 或 Linux 来宾操作系统可用性和性能的功能。 Azure Monitor 为关键组件及其关系、有关如何度量这些组件的运行状况的条件，以及在检测到不正常情况时由哪些组件会发出警报的设置建模。 工作簿允许用户使用这些信息创建丰富的交互式报表。

若要让查询控件使用此数据源，请使用“查询类型”下拉列表选择“工作负荷运行状况”，然后选择目标订阅、资源组或 VM 资源。 使用运行状况筛选器下拉列表，根据分析需求选择有意义的运行状况事件子集。

![警报查询的屏幕截图](./media/workbooks-overview/workload-health.png)

## <a name="azure-resource-health"></a>Azure 资源运行状况 

工作簿支持获取 Azure 资源运行状况，并将其与其他数据源合并，以创建丰富的交互式运行状况报告

若要让查询控件使用此数据源，请使用“查询类型”下拉列表选择“Azure 运行状况”，然后选择目标资源。 使用运行状况筛选器下拉列表，根据分析需求选择有意义的资源问题子集。

![警报查询的屏幕截图](./media/workbooks-overview/resource-health.png)

## <a name="azure-data-explorer-preview"></a>Azure 数据资源管理器（预览版）

工作簿现在支持使用强大的 [Kusto](https://docs.microsoft.com/azure/kusto/query/index) 查询语言从 [Azure 数据资源管理器](https://docs.azure.cn/data-explorer/)群集进行查询。   
<!-- correct in MC: https://docs.azure.cn/data-explorer/ -->

![Kusto 查询窗口的屏幕截图](./media/workbooks-overview/data-explorer.png)

## <a name="next-steps"></a>后续步骤

* [开始](workbooks-visualizations.md)详细了解工作簿丰富的可视化效果选项。
* [控制](workbooks-access-control.md)并共享对工作簿资源的访问权限。

