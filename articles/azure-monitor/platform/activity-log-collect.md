---
title: 在 Azure Monitor 中收集和分析 Azure 活动日志
description: 将 Azure 活动日志收集到 Azure Monitor 日志中，并使用监视解决方案分析并搜索所有 Azure 订阅的 Azure 活动日志。
ms.subservice: logs
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 05/28/2020
origin.date: 09/30/2019
ms.openlocfilehash: 461a51328c359f1875cbbccf4fd365f4d14b2005
ms.sourcegitcommit: 5ae04a3b8e025986a3a257a6ed251b575dbf60a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2020
ms.locfileid: "84440668"
---
# <a name="collect-and-analyze-azure-activity-log-in-azure-monitor"></a>在 Azure Monitor 中收集和分析 Azure 活动日志
[Azure 活动日志](platform-logs-overview.md)是一种方便用户深入了解 Azure 中发生的订阅级别事件的[平台日志](platform-logs-overview.md)。 虽然可以在 Azure 门户中查看活动日志，但还是应将其配置为发送到 Log Analytics 工作区，以启用 Azure Monitor 的其他功能。 本文介绍如何执行此配置以及如何将活动日志发送到 Azure 存储和事件中心。

在 Log Analytics 工作区中收集活动日志有以下益处：

- Log Analytics 工作区中存储的活动日志数据不产生数据引入或数据保留费用。
- 使活动日志数据与 Azure Monitor 收集的其他监视数据产生关联。
- 使用日志查询来执行复杂分析，并深入了解活动日志条目。
- 将日志警报与活动条目配合使用，从而可以使用更复杂的警报逻辑。
- 将活动日志条目存储 90 天以上。
- 将来自多个 Azure 订阅和租户的活动日志合并到同一位置一起进行分析。

## <a name="collecting-activity-log"></a>收集活动日志
活动日志会自动收集，以供[在 Azure 门户中查看](activity-log-view.md)。 若要在 Log Analytics 工作区中收集活动日志，或将其发送到 Azure 存储或事件中心，请创建[诊断设置](diagnostic-settings.md)。 此方法与资源日志所用方法相同，这样所有[平台日志](platform-logs-overview.md)的收集方式就保持了一致性。  

若要为活动日志创建诊断设置，请从 Azure Monitor 的“活动日志”菜单中选择“诊断设置” 。 有关创建该设置的详细信息，请参阅[创建诊断设置以收集 Azure 中的平台日志和指标](diagnostic-settings.md)。 有关可筛选类别的说明，请参阅[活动日志中的类别](activity-log-view.md#categories-in-the-activity-log)。 如果有任何旧版设置，请确保在创建诊断设置之前禁用这些设置。 同时启用两者可能会导致数据重复。

![诊断设置](./media/diagnostic-settings-subscription/diagnostic-settings.png)


> [!NOTE]
> 目前，只能使用 Azure 门户和资源管理器模板创建订阅级别的诊断设置。 


## <a name="legacy-settings"></a>旧版设置 
虽然诊断设置是将活动日志发送到不同目标的首选方法，但如果不选择将旧版方法替换为诊断设置，旧版方法将继续起作用。 诊断设置与旧版方法相比具有以下优势，所以建议你更新配置：

- 用于收集所有平台日志的一致方法。
- 跨多个订阅和租户收集活动日志。
- 筛选集合，以便仅收集特定类别的日志。
- 收集所有活动日志类别。 部分类别不是使用旧版方法收集的。
- 更短的日志引入延迟。 以前的方法的延迟大约为 15 分钟，而诊断设置仅会增加约 1 分钟的延迟。



### <a name="log-profiles"></a>日志配置文件
日志配置文件是用于将活动日志发送到 Azure 存储或事件中心的旧版方法。 请使用以下过程，以继续使用日志配置文件或将其禁用（如果准备迁移到诊断设置）。

1. 从 Azure 门户上的 Azure Monitor 菜单中，选择“活动日志” 。
3. 单击“诊断设置”。

   ![诊断设置](./media/diagnostic-settings-subscription/diagnostic-settings.png)

4. 单击紫色横幅了解旧版体验。

    ![旧版体验](./media/diagnostic-settings-subscription/legacy-experience.png)

### <a name="log-analytics-workspace"></a>Log Analytics 工作区
将活动日志收集到 Log Analytics 工作区中的旧版方法是在工作区配置中连接该日志。 

1. 在 Azure 门户的“Log Analytics 工作区”菜单中，选择要收集活动日志的工作区。
1. 在工作区的菜单的“工作区数据源”部分，选择“Azure 活动日志”。 
1. 单击要连接的订阅。

    ![工作区](./media/activity-log-collect/workspaces.png)

1. 单击“连接”，将订阅中的活动日志连接到所选工作区。 如果订阅已连接到其他工作区，请先单击“断开连接”将其断开连接。

    ![连接工作区](./media/activity-log-collect/connect-workspace.png)


若要禁用该设置，请执行相同步骤，然后单击“断开连接”，以从工作区中删除该订阅。


## <a name="analyze-activity-log-in-log-analytics-workspace"></a>在 Log Analytics 工作区中分析活动日志
将活动日志连接到 Log Analytics 工作区时，条目会写入到工作区的名为 *AzureActivity* 的表中，该表可以使用[日志查询](../log-query/log-query-overview.md)进行检索。 此表的结构因[日志条目类别](activity-log-view.md#categories-in-the-activity-log)而异。 有关每个类别的说明，请参阅 [Azure 活动日志事件架构](activity-log-schema.md)。


### <a name="data-structure-changes"></a>数据结构更改
诊断设置与用于收集活动日志的旧版方法收集的数据是相同的，但 AzureActivity 表的结构存在一些更改。

更新后的架构中已经弃用了下表中的列。 这些列仍存在于 AzureActivity 中，但不会包含任何数据。 这些列的替换项并不是新的，而是包含了与已弃用列相同的数据。 它们采用了一种不同的格式，所以你可能需要对使用它们的日志查询进行修改。 

| 已弃用的列 | 替换列 |
|:---|:---|
| ActivityStatus    | ActivityStatusValue    |
| ActivitySubstatus | ActivitySubstatusValue |
| OperationName     | OperationNameValue     |
| ResourceProvider  | ResourceProviderValue  |

> [!IMPORTANT]
> 在有些情况下，这些列中的值可能全部都是大写。 如果你的某个查询包含这些列，应使用 [= ~ 运算符](https://docs.microsoft.com/azure/kusto/query/datatypes-string-operators)来执行不区分大小写的比较。

以下列已添加到更新后架构的 AzureActivity 中：

- Authorization_d
- Claims_d
- Properties_d


## <a name="activity-logs-analytics-monitoring-solution"></a>Activity Logs Analytics 监视解决方案
Azure Log Analytics 监视解决方案不久就会被弃用，并将替换为一个使用 Log Analytics 工作区中更新后架构的工作簿。 如果已经启用了该解决方案，则仍可继续使用它，但它只能在你使用旧版设置收集活动日志时使用。 



### <a name="use-the-solution"></a>使用解决方案
可以在 Azure 门户的“监视器”菜单中访问监视解决方案。 在“见解”部分选择“更多”，打开包含解决方案磁贴的“概览”页  。 “Azure 活动日志”磁贴显示工作区中 **AzureActivity** 记录的计数。

![Azure 活动日志磁贴](./media/collect-activity-logs/azure-activity-logs-tile.png)


单击“Azure 活动日志”磁贴，打开“Azure 活动日志”视图。 视图包含下表中的可视化部件。 每个部件按照指定时间范围列出了匹配该部件条件的最多 10 个项。 可通过单击部件底部的“查看全部”运行返回所有匹配记录的日志查询。

![Azure 活动日志仪表板](./media/collect-activity-logs/activity-log-dash.png)


### <a name="enable-the-solution-for-new-subscriptions"></a>为新订阅启用该解决方案
不久之后，你将无法再使用 Azure 门户将 Activity Logs Analytics 解决方案添加到你的订阅。 可通过资源管理器模板使用以下过程添加该解决方案。 

1. 将以下 json 复制到名为 ActivityLogTemplate.json 的文件中。

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "defaultValue": "my-workspace",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "east us",
              "west us",
              "australia central",
              "west europe"
            ],
            "defaultValue": "australia central",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        }
      },
        "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "features": {
                    "searchVersion": 2
                }
            }
        },
        {
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "2015-11-01-preview",
            "name": "[concat('AzureActivity(', parameters('workspaceName'),')')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]"
            ],
            "plan": {
                "name": "[concat('AzureActivity(', parameters('workspaceName'),')')]",
                "promotionCode": "",
                "product": "OMSGallery/AzureActivity",
                "publisher": "Microsoft"
            },
            "properties": {
                "workspaceResourceId": "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]",
                "containedResources": [
                    "[concat(resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName')), '/views/AzureActivity(',parameters('workspaceName'))]"
                ]
            }
        },
        {
          "type": "Microsoft.OperationalInsights/workspaces/datasources",
          "kind": "AzureActivityLog",
          "name": "[concat(parameters('workspaceName'), '/', subscription().subscriptionId)]",
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "dependsOn": [
              "[parameters('WorkspaceName')]"
          ],
          "properties": {
              "linkedResourceId": "[concat(subscription().Id, '/providers/microsoft.insights/eventTypes/management')]"
          }
        }
      ]
    }    
    ```

2. 使用以下 PowerShell 命令部署该模板：

    ```PowerShell
    Connect-AzAccount -Environment AzureChinaCloud
    Select-AzSubscription <SubscriptionName>
    New-AzResourceGroupDeployment -Name activitysolution -ResourceGroupName <ResourceGroup> -TemplateFile <Path to template file>
    ```


## <a name="next-steps"></a>后续步骤

- 详细了解[活动日志](platform-logs-overview.md)。
- 详细了解 [Azure Monitor 数据平台](data-platform.md)。
- 使用[日志查询](../log-query/log-query-overview.md)查看活动日志中的详细信息。

