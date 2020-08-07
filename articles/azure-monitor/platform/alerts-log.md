---
title: 使用 Azure Monitor 创建、查看和管理日志警报 | Azure Docs
description: 在 Azure 中使用 Azure Monitor 创作、查看和管理日志警报规则。
author: Johnnytechn
ms.author: v-johya
ms.topic: conceptual
origin.date: 4/27/2018
ms.date: 07/17/2020
ms.subservice: alerts
ms.openlocfilehash: 232b4c68a391190bcc2bc45da1d727a36667d725
ms.sourcegitcommit: b5794af488a336d84ee586965dabd6f45fd5ec6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87508479"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>使用 Azure Monitor 创建、查看和管理日志警报

## <a name="overview"></a>概述
本文演示如何使用 Azure 门户中的警报界面创建和管理日志警报。 警报规则由三个组件定义：
- 目标：要监视的特定 Azure 资源
- 条件：要计算真假的条件或逻辑。 如果为 true，则触发警报。  
- 操作：发送到通知接收方 - 电子邮件、短信、Webhook 等的特定调用。

“日志警报”这一术语描述计算 [Log Analytics 工作区](../log-query/get-started-portal.md) 或 [Application Insights](../app/analytics.md) 中的日志查询的警报，并在结果为 true 时触发警报。 从[日志警报 - 概述](alerts-unified-log.md)中详细了解功能、术语和类型。

> [!NOTE]
> 还可以将 [Log Analytics 工作区](../../azure-monitor/log-query/get-started-portal.md)的日志数据路由到 Azure Monitor 指标数据库。 指标警报具有[不同的行为](alerts-metric-overview.md)，该行为可能更可取，具体取决于你要使用的数据。   要了解如何将日志路由到指标，请参阅[日志的指标警报](alerts-metric-logs.md)。
<!--Correct in MC: ../../azure-monitor/log-query/get-started-portal.md-->

## <a name="create-a-log-alert-rule-with-the-azure-portal"></a>使用 Azure 门户创建日志警报规则

1. 在[门户](https://portal.azure.cn/)中，选择“监视器”。 在该部分中，选择“警报”。

    ![监视](./media/alerts-log/AlertsPreviewMenu.png)

1. 单击“新建警报规则”。 

    ![添加警报](./media/alerts-log/AlertsPreviewOption.png)

1. 随即显示“创建警报”窗格。 它有四个部分： 
    - 应用警报的资源
    - 要检查的条件
    - 条件为 true 时要执行的操作
    - 命名和描述警报的详细信息。 

    ![创建规则](./media/alerts-log/AlertsPreviewAdd.png)

1. 定义警报条件：使用“选择资源”链接，然后通过选择资源来指定目标。 进行筛选：选择“订阅”和“资源类型”，以及所需的资源。   

   ![选择资源](./media/alerts-log/Alert-SelectResourceLog.png)

1. 确保“资源类型”是分析源（例如“Log Analytics”或“Application Insights”），且信号类型为“日志”  。 单击“Done”（完成） 。 接下来，使用“添加条件”按钮查看可用于该资源的信号选项列表。 查找并选择针对“Log Analytics”或“Application Insights”的“自定义日志搜索”选项，具体取决于日志警报的数据所在的位置 。

   ![选择资源 - 自定义日志搜索](./media/alerts-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]
   > 
   > 警报列表可以导入分析查询作为信号类型 - **日志（已保存查询）** ，如上图所示。 因此，用户可以在 Analytics 中优化查询，然后保存这些查询以供将来在警报中使用。 有关使用已保存的查询的更多详细信息，请参阅[在 Azure Monitor 中使用日志查询](../log-query/log-query-overview.md)和 [Application Insights Analytics 中的共享查询](../app/app-insights-overview.md)。

1. 选择后，在“搜索查询”字段中创建警报查询。 如果查询语法不正确，则字段会以红色显示错误。 

1. 如果查询语法正确，将以图表形式显示查询的历史数据，同时显示用于调整时间范围（从过去六小时到过去一周）的选项。

    ![配置警报规则](./media/alerts-log/AlertsPreviewAlertLog.png)

   仅当查询结果包含时间详细信息时，才能显示历史数据可视化效果。 如果查询生成了汇总数据或特定列值，则显示单一绘图。
  
   对于使用 Application Insights 或 [Log Analytics API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) 的指标度量值，可以使用“聚合依据”选项指定要使用哪个特定变量对数据进行分组，如下面所示： 
  
   ![“聚合基于”选项](./media/alerts-log/aggregate-on.png)



1. 接下来，选择“警报逻辑”条件、聚合和阈值。 

1. 使用“时段”选项，选择评估指定条件的时段。 

1. 在“频率”中选择警报运行的频率。 

    日志警报可以基于：
    - [记录数目](../../azure-monitor/platform/alerts-unified-log.md#number-of-results-alert-rules)：如果查询返回的记录计数大于或小于提供的值，则创建警报。
    - [指标度量](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules)：如果结果中的每个聚合值超过提供的阈值并且是“分组依据”选定值，则创建警报 。 警报违规数是在选定时间段内超过阈值的次数。 可以为结果集中的任何违规组合指定总违规数，或指定连续违规数以要求违规必须在连续采样时发生。


1. 单击“Done”（完成） 。 

1. 在“警报规则名称”字段中定义警报的名称，以及用于详细描述该警报细节和提供的选项中的“严重性”值的“说明”  。 在 Azure Monitor 发送的所有警报电子邮件、通知或推送内容中，将重用这些详细信息。 此外，你可以通过点击“创建后启用规则”，选择在创建后立即激活该警报规则。

1. 选择是否要“取消显示警报”一段时间。  如果打开警报规则的阻止功能，则新建警报之后会在定义的时间段内禁用该规则的操作。 此规则仍在运行中，并且会在满足条件的情况下创建警报记录。 该设置让你有时间更正问题，而无需运行重复操作。

   ![对日志警报禁止显示警报](./media/alerts-log/AlertsPreviewSuppress.png)

    > [!TIP]
    > 将禁止显示警报值指定为大于警报频率，以确保在没有重叠的情况下停止通知

1. 第三个步骤（也是最后一个步骤）指定警报规则是否应在满足警报条件时触发一个或多个操作组。 可以选择任何现有的操作组或创建新组。 使用操作组，可以发送多个操作，例如发送电子邮件、发送短信、调用 Webhook、使用 Azure Runbook 进行修正、推送到 ITSM 工具等。 详细了解[操作组](action-groups.md)。

    > [!NOTE]
    > 有关可以执行的操作的限制，请参阅 [Azure 订阅服务限制](../../azure-resource-manager/management/azure-subscription-service-limits.md)。  

    提供了一些附加功能用于替代默认操作：

    - **电子邮件通知**：替代通过操作组发送的电子邮件中的电子邮件主题。 无法修改邮件正文，并且该字段不能用于电子邮件地址。
    - **包含自定义 JSON 有效负载**：假定操作组包含 Webhook 类型，则替代操作组使用的 Webhook JSON。 有关 Webhook 格式的详细信息，请参阅[针对日志警报的 Webhook 操作](../../azure-monitor/platform/alerts-log-webhook.md)。 提供了“查看 Webhook”选项来使用示例 JSON 数据检查格式。

        ![日志警报的操作替代](./media/alerts-log/AlertsPreviewOverrideLog.png)


1. 如果所有字段有效并且附带绿色的勾选标记，则可以单击“创建警报规则”按钮，在“Azure Monitor - 警报”中创建警报。 可以从警报仪表板查看所有警报。

     ![创建规则](./media/alerts-log/AlertsPreviewCreate.png)

     在几分钟后，警报将如前所述激活并触发。

用户还可以在 [Log Analytics](../log-query/portals.md) 中完成其分析查询，然后推送该查询，以通过“设置警报”按钮，遵循以上教程中从步骤 6 开始往后的说明创建警报。

 ![Log Analytics - 设置警报](./media/alerts-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>在 Azure 门户中查看和管理日志警报

1. 在[门户](https://portal.azure.cn/)中选择“监视器”，然后在“监视器”部分下选择“警报”。 

1. **此时将显示警报仪表板** - 其中，所有 Azure 警报（包括日志警报）都显示在单个面板中；包括你的日志警报规则触发时间的每个实例。 若要了解详细信息，请参阅[警报管理](https://aka.ms/managealertinstances)。
    > [!NOTE]
    > 日志警报规则包括由用户提供的基于自定义查询的逻辑，因此不存在已解决状态。 因此，每当满足日志警报规则中指定的条件时，它都会触发。

1. 在顶部栏中选择“管理规则”按钮，导航到规则管理部分 - 其中列出了创建的所有警报规则，包括已禁用的警报。
    ![管理警报规则](./media/alerts-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>使用 Azure 资源模板管理日志警报

Azure Monitor 中的日志警报与资源类型 `Microsoft.Insights/scheduledQueryRules/` 相关联。 有关此资源类型的详细信息，请参阅 [Azure Monitor - 计划查询规则 API 参考](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/)。 可以使用[计划查询规则 API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) 为 Application Insights 或 Log Analytics 创建日志警报。

> [!NOTE]
> 还可以使用旧式 [Log Analytics 警报 API](api-alerts.md) 以及 [Log Analytics 保存的搜索和警报](../insights/solutions-resources-searches-alerts.md)的旧式模板管理 Log Analytics 的日志警报。
<!--Currently is not available in MC: alerts-log-api-switch.md-->


### <a name="sample-log-alert-creation-using-azure-resource-template"></a>使用 Azure 资源模板创建日志警报示例

以下是基于资源模板的[计划查询规则创建](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate)结构，它使用[结果类型日志警报的数量](alerts-unified-log.md#number-of-results-alert-rules)的标准日志搜索查询，其中示例数据集作为变量。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {
        "alertLocation": "chinaeast2",
        "alertName": "samplelogalert",
        "alertDescription": "Sample log search alert",
        "alertStatus": "true",
        "alertSource":{
            "Query":"requests",
            "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
            "Type":"ResultCount"
        },
        "alertSchedule":{
            "Frequency": 15,
            "Time": 60
        },
        "alertActions":{
            "SeverityLevel": "4"
        },
        "alertTrigger":{
            "Operator":"GreaterThan",
            "Threshold":"1"
        },
        "actionGrp":{
            "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
            "Subject": "Customized Email Header",
            "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"
        }
    },
    "resources":[ {
        "name":"[variables('alertName')]",
        "type":"Microsoft.Insights/scheduledQueryRules",
        "apiVersion": "2018-04-16",
        "location": "[variables('alertLocation')]",
        "properties":{
            "description": "[variables('alertDescription')]",
            "enabled": "[variables('alertStatus')]",
            "source": {
                "query": "[variables('alertSource').Query]",
                "dataSourceId": "[variables('alertSource').SourceId]",
                "queryType":"[variables('alertSource').Type]"
            },
            "schedule":{
                "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
                "timeWindowInMinutes": "[variables('alertSchedule').Time]"
            },
            "action":{
                "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
                "severity":"[variables('alertActions').SeverityLevel]",
                "aznsAction":{
                    "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
                    "emailSubject":"[variables('actionGrp').Subject]",
                    "customWebhookPayload":"[variables('actionGrp').Webhook]"
                },
                "trigger":{
                    "thresholdOperator":"[variables('alertTrigger').Operator]",
                    "threshold":"[variables('alertTrigger').Threshold]"
                }
            }
        }
    } ]
}

```

针对此演练，上面的示例 json 可以保存为（例如）sampleScheduledQueryRule.json，并且可以使用 [Azure 门户中的 Azure 资源管理器](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)进行部署。


### <a name="log-alert-with-cross-resource-query-using-azure-resource-template"></a>使用 Azure 资源模板进行跨资源查询的日志警报

以下是基于资源模板的[计划查询规则创建](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate)结构，它使用[指标度量值类型日志警报](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules)的[跨资源日志搜索查询](../../azure-monitor/log-query/cross-workspace-query.md)，其中示例数据集作为变量。

```json

{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {
        "alertLocation": "Region Name for your Application Insights App or Log Analytics Workspace",
        "alertName": "sample log alert",
        "alertDescr": "Sample log search alert",
        "alertStatus": "true",
        "alertSource":{
            "Query":"union workspace(\"servicews\").Update, app('serviceapp').requests | summarize AggregatedValue = count() by bin(TimeGenerated,1h), Classification",
            "Resource1": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
            "Resource2": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/components/serviceapp",
            "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
            "Type":"ResultCount"
        },
        "alertSchedule":{
            "Frequency": 15,
            "Time": 60
        },
        "alertActions":{
            "SeverityLevel": "4",
            "SuppressTimeinMin": 20
        },
        "alertTrigger":{
            "Operator":"GreaterThan",
            "Threshold":"1"
        },
        "metricMeasurement": {
            "thresholdOperator": "Equal",
            "threshold": "1",
            "metricTriggerType": "Consecutive",
            "metricColumn": "Classification"
        },
        "actionGrp":{
            "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG",
            "Subject": "Customized Email Header",
            "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"
        }
    },
    "resources":[ {
        "name":"[variables('alertName')]",
        "type":"Microsoft.Insights/scheduledQueryRules",
        "apiVersion": "2018-04-16",
        "location": "[variables('alertLocation')]",
        "properties":{
            "description": "[variables('alertDescr')]",
            "enabled": "[variables('alertStatus')]",
            "source": {
                "query": "[variables('alertSource').Query]",
                "authorizedResources": "[concat(array(variables('alertSource').Resource1), array(variables('alertSource').Resource2))]",
                "dataSourceId": "[variables('alertSource').SourceId]",
                "queryType":"[variables('alertSource').Type]"
            },
            "schedule":{
                "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
                "timeWindowInMinutes": "[variables('alertSchedule').Time]"
            },
            "action":{
                "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
                "severity":"[variables('alertActions').SeverityLevel]",
                "throttlingInMin": "[variables('alertActions').SuppressTimeinMin]",
                "aznsAction":{
                    "actionGroup": "[array(variables('actionGrp').ActionGroup)]",
                    "emailSubject":"[variables('actionGrp').Subject]",
                    "customWebhookPayload":"[variables('actionGrp').Webhook]"
                },
                "trigger":{
                    "thresholdOperator":"[variables('alertTrigger').Operator]",
                    "threshold":"[variables('alertTrigger').Threshold]",
                    "metricTrigger":{
                        "thresholdOperator": "[variables('metricMeasurement').thresholdOperator]",
                        "threshold": "[variables('metricMeasurement').threshold]",
                        "metricColumn": "[variables('metricMeasurement').metricColumn]",
                        "metricTriggerType": "[variables('metricMeasurement').metricTriggerType]"
                    }
                }
            }
        }
    } ]
}

```

> [!IMPORTANT]
> 使用日志警报中的跨资源查询时，必须使用 [authorizedResources](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate#source) 且用户必须有权访问所述的资源列表

针对此演练，上面的示例 json 可以保存为（例如）sampleScheduledQueryRule.json，并且可以使用 [Azure 门户中的 Azure 资源管理器](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)进行部署。

## <a name="managing-log-alerts-using-powershell"></a>使用 PowerShell 管理日志警报

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor - [计划查询规则 API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) 是一个 REST API，与 Azure 资源管理器 REST API 完全兼容。 下面列出的 PowerShell cmdlet 可供利用[计划查询规则 API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/)。

- [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule)：用于新建日志警报规则的 PowerShell cmdlet。
- [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule)：用于更新现有日志警报规则的 PowerShell cmdlet。
- [New-AzScheduledQueryRuleSource](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrulesource)：用于创建或更新对象的 PowerShell cmdlet，该对象为日志警报指定源参数。 由 [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) 和 [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet 用作输入。
- [New-AzScheduledQueryRuleSchedule](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleSchedule)：用于创建或更新对象的 PowerShell cmdlet，该对象为日志警报指定计划参数。 由 [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) 和 [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet 用作输入。
- [New-AzScheduledQueryRuleAlertingAction](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleAlertingAction)：用于创建或更新对象的 PowerShell cmdlet，该对象为日志警报指定操作参数。 由 [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) 和 [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet 用作输入。
- [New-AzScheduledQueryRuleAznsActionGroup](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryruleaznsactiongroup)：用于创建或更新对象的 PowerShell cmdlet，该对象为日志警报指定操作组参数。 由 [New-AzScheduledQueryRuleAlertingAction](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleAlertingAction) cmdlet 用作输入。
- [New-AzScheduledQueryRuleTriggerCondition](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition)：用于创建或更新对象的 PowerShell cmdlet，该对象为日志警报指定触发器条件参数。 由 [New-AzScheduledQueryRuleAlertingAction](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleAlertingAction) cmdlet 用作输入。
- [New-AzScheduledQueryRuleLogMetricTrigger](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrulelogmetrictrigger)：用于创建或更新对象的 PowerShell cmdlet，该对象为[指标度量值类型日志警报](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules)指定触发器条件参数。 由 [New-AzScheduledQueryRuleTriggerCondition](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition) cmdlet 用作输入。
- [Get-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/get-azscheduledqueryrule)：用于列出现有日志警报规则或指定日志警报规则的 PowerShell cmdlet
- [Update-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/update-azscheduledqueryrule)：用于启用或禁用日志警报规则的 PowerShell cmdlet
- [Remove-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/remove-azscheduledqueryrule)：用于删除现有日志警报规则的 PowerShell cmdlet

> [!NOTE]
> ScheduledQueryRules PowerShell cmdlet 只能管理使用 cmdlet 本身或 Azure Monitor - [计划查询规则 API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) 创建的规则。
<!--Currently is not available in MC: alerts-log-api-switch.md-->

下面演示了使用 scheduledQueryRules PowerShell cmdlet 创建示例日志警报规则的步骤。

```powershell
$source = New-AzScheduledQueryRuleSource -Query 'Heartbeat | summarize AggregatedValue = count() by bin(TimeGenerated, 5m), _ResourceId' -DataSourceId "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews"

$schedule = New-AzScheduledQueryRuleSchedule -FrequencyInMinutes 15 -TimeWindowInMinutes 30

$metricTrigger = New-AzScheduledQueryRuleLogMetricTrigger -ThresholdOperator "GreaterThan" -Threshold 2 -MetricTriggerType "Consecutive" -MetricColumn "_ResourceId"

$triggerCondition = New-AzScheduledQueryRuleTriggerCondition -ThresholdOperator "LessThan" -Threshold 5 -MetricTrigger $metricTrigger

$aznsActionGroup = New-AzScheduledQueryRuleAznsActionGroup -ActionGroup "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG" -EmailSubject "Custom email subject" -CustomWebhookPayload "{ `"alert`":`"#alertrulename`", `"IncludeSearchResults`":true }"

$alertingAction = New-AzScheduledQueryRuleAlertingAction -AznsAction $aznsActionGroup -Severity "3" -Trigger $triggerCondition

New-AzScheduledQueryRule -ResourceGroupName "contosoRG" -Location "Region Name for your Application Insights App or Log Analytics Workspace" -Action $alertingAction -Enabled $true -Description "Alert description" -Schedule $schedule -Source $source -Name "Alert Name"
```

## <a name="managing-log-alerts-using-cli-or-api"></a>使用 CLI 或 API 管理日志警报

Azure Monitor - [计划查询规则 API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) 是一个 REST API，与 Azure 资源管理器 REST API 完全兼容。 因此，可使用 Azure CLI 的资源管理器命令，通过 Powershell 来利用它。


> [!NOTE]
> 还可以使用旧式 [Log Analytics 警报 API](api-alerts.md) 以及 [Log Analytics 保存的搜索和警报](../insights/solutions-resources-searches-alerts.md)的旧式模板管理 Log Analytics 的日志警报。
<!--Currently is not available in MC: alerts-log-api-switch.md-->

日志警报目前没有专用的 CLI 命令；但是如下所示，这些警报可通过用于“资源模板”部分前面显示的示例资源模板 (sampleScheduledQueryRule.json) 的 Azure 资源管理器 CLI 命令来使用：

```azurecli
az group deployment create --resource-group contosoRG --template-file sampleScheduledQueryRule.json
```

成功执行操作后，将返回 201 声明新的警报规则创建，如果修改了现有警报规则，则返回 200。

## <a name="next-steps"></a>后续步骤

* 了解 [Azure 警报中的日志警报](../../azure-monitor/platform/alerts-unified-log.md)
* 了解[用于日志警报的 Webhook 操作](../../azure-monitor/platform/alerts-log-webhook.md)
* 详细了解 [Application Insights](../../azure-monitor/log-query/log-query-overview.md)
* 了解有关[日志查询](../log-query/log-query-overview.md)的详细信息。
<!--Correct in MC: ../../azure-monitor/log-query/log-query-overview.md-->
