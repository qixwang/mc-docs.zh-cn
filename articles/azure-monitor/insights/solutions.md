---
title: Azure Monitor 中的监视解决方案 | Azure Docs
description: Azure Monitor 中的监视解决方案是逻辑、可视化效果和数据采集规则的集合，提供围绕特定问题领域制定的指标。  本文提供了有关安装和使用监视解决方案的信息。
ms.subservice: logs
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
origin.date: 08/13/2019
ms.date: 05/28/2020
ms.openlocfilehash: 92ae4c09ff5d8dc0fdf1dde53656f6e76c669163
ms.sourcegitcommit: 5ae04a3b8e025986a3a257a6ed251b575dbf60a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2020
ms.locfileid: "84440720"
---
# <a name="monitoring-solutions-in-azure-monitor"></a>Azure Monitor 中的监视解决方案

监视解决方案利用 Azure 中的服务来提供特定应用程序或服务的更多操作见解。 本文简要概述了 Azure 中的监视解决方案，并详细介绍如何了使用和安装这些解决方案。

> [!NOTE]
> 监视解决方案以前称为管理解决方案。

监视解决方案通常收集日志数据并提供查询和视图，用于分析收集的数据。 这些解决方案还可以利用 Azure 自动化等其他服务来执行与应用程序或服务相关的操作。

可以在 Azure Monitor 中针对你使用的任何应用程序和服务添加监视解决方案。 这些解决方案通常是免费提供的，但收集的数据可能会产生使用费用。 除了由 Microsoft 提供的解决方案以外，合作伙伴和客户还可以[创建管理解决方案](solutions-creating.md)，以便在各自环境中使用或通过社区提供给客户使用。

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

<!-- Not available in MC: Open the **Overview** page in Azure Monitor to display a tile for each solution installed in the workspace.
Monitoring solutions can contain multiple types of Azure resources, and you can view any resources included with a solution just like any other resource. For example, any log queries included in the solution are listed under **Solution Queries** in [Query explorer](../log-query/get-started-portal.md#load-queries) You can use those queries when performing ad hoc analysis with [log queries](../log-query/log-query-overview.md).

## List installed monitoring solutions

Use the following procedure to list the monitoring solutions installed in your subscription.

1. Go to the [Azure portal](https://portal.azure.cn). Search for and select **Solutions**.
1. Solutions installed in all your workspaces are listed. The name of the solution is followed by the name of the workspace it's installed in.
1. Use the dropdown boxes at the top of the screen to filter by subscription or resource group.


![List all solutions](./media/solutions/list-solutions-all.png)

Click on the name of a solution to open its summary page. This page displays any views included in the solution and provides different options for the solution itself and its workspace. View the summary page for a solution by using one of the procedures above to list solutions and then click on the name of the solution.

![Solution properties](./media/solutions/solution-properties.png)

## Install a monitoring solution
Monitoring solutions from Azure and partners are available from the [Azure Marketplace](https://market.azure.cn). You can search available solutions and install them using the following procedure. When you install a solution, you must select a [Log Analytics workspace](../platform/manage-access.md) where the solution will be installed and where its data will be collected.

1. From the list of solutions for your subscription, click **Add**.
1. Browse or search for a solution. You can also browse solutions from [this search link](https://market.azure.cn/zh-cn/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).
1. Locate the monitoring solution you want and read through its description.
1. Click **Create** to start the installation process.
1. When the installation process starts, you're prompted to specify the Log Analytics workspace and provide any required configuration for the solution.
<!--Correct in MC: https://market.azure.cn/zh-cn/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions -->

![安装解决方案](./media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>从社区安装解决方案

社区成员可以将管理解决方案提交到 Azure 快速入门模板。 可以直接安装这些解决方案，或者下载模板，以便今后安装。

1. 请遵循 [Log Analytics 工作区和自动化帐户](#log-analytics-workspace-and-automation-account)中所述的过程来链接工作区和帐户。
2. 转到 [Azure 快速入门模板](https://azure.microsoft.com/documentation/templates/)。 
3. 搜索感兴趣的解决方案。
4. 从结果中选择解决方案以查看其详细信息。
5. 单击“部署到 Azure”按钮。
6. 除了解决方案中任何参数的值，系统还会提示提供资源组和位置等信息。
7. 单击“**购买**”可安装解决方案。

## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics 工作区和自动化帐户

所有监视解决方案都需要使用一个 [Log Analytics 工作区](../platform/manage-access.md)来存储解决方案收集的数据，以及托管其日志搜索和视图。 某些解决方案还需要使用一个[自动化帐户](../../automation/automation-security-overview.md#automation-account-overview)来包含 Runbook 和相关资源。 工作区和帐户必须满足以下要求。

* 解决方案的每项安装只能使用一个 Log Analytics 工作区和一个自动化帐户。 可将解决方案单独安装到多个工作区。
* 如果解决方案需要自动化帐户，则必须将 Log Analytics 工作区和自动化帐户相互链接。 一个 Log Analytics 工作区只能链接到一个自动化帐户，而一个自动化帐户也只能链接到一个 Log Analytics 工作区。
* 若要进行链接，Log Analytics 工作区和自动化帐户必须位于同一订阅中，但可以位于部署到同一区域的不同资源组中。
<!-- Correct in MC： To be linked... -->

通过 Azure 市场安装解决方案时，系统会提示提供一个工作区和自动化帐户。 如果工作区与自动化帐户之间尚未建立链接，则系统会创建这种链接。

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>验证 Log Analytics 工作区和自动化帐户之间的链接

可以使用以下过程验证 Log Analytics 工作区和自动化帐户之间的链接。

1. 在 Azure 门户中选择自动化帐户。
1. 滚动到菜单的“相关资源”部分，然后选择“链接的工作区”。
1. 如果“工作区”已链接到自动化帐户，则此页会列出它链接到的工作区。 如果选择列出的工作区的名称，则会重定向到该工作区的概览页。

## <a name="remove-a-monitoring-solution"></a>删除监视解决方案

若要删除已安装的解决方案，请在已安装的解决方案列表中找到它。 单击解决方案的名称打开其摘要页，然后单击“删除”。

## <a name="next-steps"></a>后续步骤

* 获取 [Microsoft 提供的监视解决方案的列表](solutions-inventory.md)。
* 了解如何[创建查询](../log-query/log-query-overview.md)来分析监视解决方案收集的数据。

