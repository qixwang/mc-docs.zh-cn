---
title: Azure Monitor 中的监视解决方案 | Azure Docs
description: Azure Monitor 中的监视解决方案是逻辑、可视化效果和数据采集规则的集合，提供围绕特定问题领域制定的指标。  本文提供了有关安装和使用监视解决方案的信息。
ms.subservice: logs
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
origin.date: 08/13/2019
ms.date: 07/17/2020
ms.openlocfilehash: d8edde32cd991baa8d89cea61c7592822401abf8
ms.sourcegitcommit: 2b78a930265d5f0335a55f5d857643d265a0f3ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87244977"
---
# <a name="monitoring-solutions-in-azure-monitor"></a>Azure Monitor 中的监视解决方案
监视解决方案利用 Azure 中的服务来提供特定应用程序或服务的更多操作分析。 本文简要概述了 Azure 中的监视解决方案，并详细介绍如何了使用和安装这些解决方案。 可以在 Azure Monitor 中针对你使用的任何应用程序和服务添加监视解决方案。 这些解决方案通常是免费的，但收集的数据可能会产生使用费。

## <a name="use-monitoring-solutions"></a>使用监视解决方案

<!--Not available in MC: **Overview** page in Azure Monitor-->
1. 转到 [Azure 门户](https://portal.azure.cn)。 搜索并选择“Monitor”。
1. 在“见解”菜单下选择“更多” 。
1. 使用屏幕顶部的下拉框更改工作区或用于磁贴的时间范围。
1. 单击解决方案的磁贴，以打开它的视图，其中包含它收集的数据的更详细分析。

![概述](./media/solutions/overview.png)

监视解决方案可以包含多种类型的 Azure 资源。可以像查看其他任何资源一样查看解决方案包含的任何资源。 例如，解决方案中包含的任何日志查询都列在[查询资源管理器](../log-query/get-started-portal.md#load-queries)中的“解决方案查询”下，可在使用[日志查询](../log-query/log-query-overview.md)执行临时分析时使用这些查询。

## <a name="list-installed-monitoring-solutions"></a>列出已安装的监视解决方案

使用以下过程列出订阅中安装的监视解决方案。

1. 转到 [Azure 门户](https://portal.azure.cn)。 搜索并选择“解决方案”。
1. 将列出所有工作区中安装的解决方案。 解决方案名称的后面是在其中安装该解决方案的工作区的名称。
1. 使用屏幕顶部的下拉框可按订阅或资源组进行筛选。


![列出所有解决方案](./media/solutions/list-solutions-all.png)

单击某个解决方案的名称打开其摘要页。 此页显示解决方案中包含的所有视图，并提供解决方案本身及其工作区的不同选项。 使用上述过程之一查看解决方案的摘要页以列出解决方案，然后单击解决方案的名称。

![解决方案属性](./media/solutions/solution-properties.png)

## <a name="install-a-monitoring-solution"></a>安装监视解决方案

[Azure 市场](https://market.azure.cn/)中提供了 Microsoft 和合作伙伴提供的监视解决方案。 可以搜索可用的解决方案，并使用以下过程进行安装。 安装解决方案时，必须选择要在其中安装该解决方案的 [Log Analytics 工作区](../platform/manage-access.md)，以及要将解决方案数据收集到的位置。

1. 在[订阅的解决方案列表](#list-installed-monitoring-solutions)中，单击“添加”。
1. 浏览或搜索解决方案。 
1. 找到所需的监视解决方案并阅读其说明。
1. 单击“创建”以启动安装进程。
1. 当安装过程开始时，系统会提示你指定 Log Analytics 工作区，并为解决方案提供任何所需的配置。

![安装解决方案](./media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>从社区安装解决方案

社区成员可以将管理解决方案提交到 Azure 快速入门模板。 可以直接安装这些解决方案，或者下载模板，以便今后安装。

1. 请遵循 [Log Analytics 工作区和自动化帐户](#log-analytics-workspace-and-automation-account)中所述的过程来链接工作区和帐户。
2. 转到 [Azure 快速入门模板](https://azure.microsoft.com/documentation/templates/)。 
3. 搜索感兴趣的解决方案。
4. 从结果中选择解决方案以查看其详细信息。
5. 单击“**部署到 Azure**”按钮。
6. 除了解决方案中任何参数的值，系统还会提示提供资源组和位置等信息。
7. 单击“**购买**”可安装解决方案。

## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics 工作区和自动化帐户

所有监视解决方案都需要使用一个 [Log Analytics 工作区](../platform/manage-access.md)来存储解决方案收集的数据，以及托管其日志搜索和视图。 某些解决方案还需要使用一个[自动化帐户](../../automation/automation-security-overview.md)来包含 Runbook 和相关资源。 工作区和帐户必须满足以下要求。

* 解决方案的每项安装只能使用一个 Log Analytics 工作区和一个自动化帐户。 可将解决方案单独安装到多个工作区。
* 如果解决方案需要自动化帐户，则必须将 Log Analytics 工作区和自动化帐户相互链接。 一个 Log Analytics 工作区只能链接到一个自动化帐户，而一个自动化帐户也只能链接到一个 Log Analytics 工作区。
* 若要进行链接，Log Analytics 工作区和自动化帐户必须位于同一订阅中，但可以位于部署到同一区域的不同资源组中。
<!-- Correct in MC： To be linked... -->

通过 Azure 市场安装解决方案时，系统会提示提供一个工作区和自动化帐户。 如果工作区与自动化帐户之间尚未建立链接，则系统会创建这种链接。

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>验证 Log Analytics 工作区和自动化帐户之间的链接

可以使用以下过程验证 Log Analytics 工作区和自动化帐户之间的链接。

1. 在 Azure 门户中选择自动化帐户。
1. 滚动到菜单的“相关资源”部分，然后选择“链接的工作区”。
1. 如果工作区已链接到自动化帐户，此页面会列出它链接到的工作区。 如果你选择所列出工作区的名称，则会被重定向到此工作区的概述页面。

## <a name="remove-a-monitoring-solution"></a>删除监视解决方案

若要删除已安装的解决方案，请在[已安装的解决方案列表](#list-installed-monitoring-solutions)中找到它。 单击解决方案的名称打开其摘要页，然后单击“删除”。

## <a name="next-steps"></a>后续步骤

* 获取 [Microsoft 提供的监视解决方案的列表](solutions-inventory.md)。
* 了解如何[创建查询](../log-query/log-query-overview.md)来分析监视解决方案收集的数据。

