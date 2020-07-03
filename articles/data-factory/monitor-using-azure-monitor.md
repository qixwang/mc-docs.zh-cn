---
title: 使用 Azure Monitor 监视数据工厂
description: 了解如何在 Azure Monitor 中，使用数据工厂中的信息通过启用诊断日志来监视 Azure 数据工厂管道。
services: data-factory
documentationcenter: ''
author: WenJason
ms.author: v-jay
manager: digimobile
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
origin.date: 12/11/2018
ms.date: 06/29/2020
ms.openlocfilehash: 5a009e88864a82a6a2a437b65ea421abf4318894
ms.sourcegitcommit: f5484e21fa7c95305af535d5a9722b5ab416683f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85323306"
---
# <a name="monitor-and-alert-data-factory-by-using-azure-monitor"></a>使用 Azure Monitor 监视数据工厂和发警报

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

云应用程序比较复杂，包含很多移动部件。 监视功能可以提供数据来确保应用程序始终处于正常运行状态。 监视功能还有助于避免潜在问题，或者排查以往的问题。 可以使用监视数据深入了解应用程序的情况。 了解这些情况有助于改进应用程序的性能或可维护性。 此外，还有助于实现本来需要手动干预的操作的自动化。

Azure Monitor 针对大多数 Azure 服务提供基本级别的基础结构指标和日志。 Azure 诊断日志由资源发出，提供与该资源的操作相关的各种频繁生成的数据。 Azure 数据工厂可以在 Azure Monitor 中写入诊断日志。 

有关更多详细信息，请参阅 [Azure Monitor 概述](/azure-monitor/overview)。

## <a name="keeping-azure-data-factory-metrics-and-pipeline-run-data"></a>保留 Azure 数据工厂指标和管道运行数据

数据工厂仅将管道运行数据存储 45 天。 若要将这些数据保留更长时间，请使用 Azure Monitor。 使用 Monitor，可以将诊断日志路由到多个不同目标进行分析。

* **存储帐户**：将诊断日志保存到存储帐户进行审核或手动检查。 可以使用诊断设置指定保留时间（天）。
* **事件中心**：将日志流式传输到 Azure 事件中心。 日志可用作合作伙伴服务或自定义分析解决方案（例如 Power BI）的输入。
* **Log Analytics**：使用 Log Analytics 分析日志。 在以下情况下，将数据工厂与 Azure Monitor 集成非常有用：
  * 需要针对由数据工厂发布到 Monitor 的丰富指标集编写复杂查询。 可以通过 Monitor 创建针对这些查询的自定义警报。
  * 你希望跨数据工厂进行监视。 可将来自多个数据工厂的数据路由到单个 Monitor 工作区。

还可使用与发出日志的资源不同的订阅中的存储帐户或事件中心命名空间。 配置此设置的用户必须对两个订阅都具有适当的基于角色的访问控制 (RBAC) 访问权限。

## <a name="configure-diagnostic-settings-and-workspace"></a>配置诊断设置和工作区

为数据工厂创建或添加诊断设置。

1. 在门户中，转到“数据工厂”。 选择“诊断设置”。

1. 选择“添加诊断设置”。

   ![如果不存在任何设置，请创建一个诊断设置](media/data-factory-monitor-oms/monitor-oms-image1.png)

1. 为设置指定名称，选择“发送到 Log Analytics”，然后从 **Log Analytics 工作区**中选择一个工作区。

   ![命名设置并选择 log-analytics 工作区](media/data-factory-monitor-oms/monitor-oms-image2.png)

1. 选择“保存” 。

几分钟后，新设置将出现在此数据工厂的设置列表中。 生成新的事件数据后，诊断日志将立即流式传输到该工作区。 发出事件后可能需要最多 15 分钟的时间该事件才会出现在 Log Analytics 中。

## <a name="data-factory-metrics"></a>数据工厂指标

使用 Monitor 可以洞察 Azure 工作负荷的性能与运行状况。 最重要的 Monitor 数据类型是指标（也称为性能计数器）。 大多数 Azure 资源都会发出指标。 Monitor 提供多种方式来配置和使用这些指标，以便进行监视与故障排除。

Azure 数据工厂版本 2 发出以下指标。

| **指标**           | **指标显示名称**         | **单位** | **聚合类型** | **说明**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRuns | 成功的管道运行数指标 | 计数    | 总计                | 在一分钟时段内成功的管道运行总数。 |
| PipelineFailedRuns   | 失败的管道运行数指标    | 计数    | 总计                | 在一分钟时段内失败的管道运行总数。    |
| ActivitySucceededRuns | 成功的活动运行数指标 | 计数    | 总计                | 在一分钟时段内成功的活动运行总数。  |
| ActivityFailedRuns   | 失败的活动运行数指标    | 计数    | 总计                | 在一分钟时段内失败的活动运行总数。     |
| TriggerSucceededRuns | 成功的触发器运行数指标  | 计数    | 总计                | 在一分钟时段内成功的触发器运行总数。   |
| TriggerFailedRuns    | 失败的触发器运行数指标     | 计数    | 总计                | 在一分钟时段内失败的触发器运行总数。      |

若要访问指标，请参阅 [Azure Monitor 数据平台](/azure-monitor/platform/data-platform)中的说明。

> [!NOTE]
> 仅发出已完成、已触发的活动和管道运行事件。 **不**会发出正在进行的运行和沙盒/调试运行。 

## <a name="data-factory-alerts"></a>数据工厂警报

登录到 Azure 门户，选择“Monitor” > “警报”以创建警报。 

![门户菜单中的警报](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>创建警报

1. 选择“+ 创建新的预警规则”，创建新的警报。

    ![新建警报规则](media/monitor-using-azure-monitor/alerts_image4.png)

1. 定义警报条件。

    > [!NOTE]
    > 请务必在“按资源类型筛选”下拉列表中选择“所有”。 

    ![“定义警报条件”>“选择目标”，此时会打开“选择资源”窗格 ](media/monitor-using-azure-monitor/alerts_image5.png)

    ![“定义警报条件”>“添加条件”，此时会打开“配置信号逻辑”窗格](media/monitor-using-azure-monitor/alerts_image6.png)

    ![“配置信号类型”窗格](media/monitor-using-azure-monitor/alerts_image7.png)

1. 定义警报详细信息。

    ![警报详细信息](media/monitor-using-azure-monitor/alerts_image8.png)

1. 定义操作组。

    ![创建规则的屏幕截图，其中突出显示了“新建操作组”](media/monitor-using-azure-monitor/alerts_image9.png)

    ![创建新的操作组](media/monitor-using-azure-monitor/alerts_image10.png)

    ![配置电子邮件、短信、推送和语音](media/monitor-using-azure-monitor/alerts_image11.png)

    ![删除操作组](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="set-up-diagnostic-logs-via-the-azure-monitor-rest-api"></a>通过 Azure Monitor REST API 设置诊断日志

### <a name="diagnostic-settings"></a>诊断设置

可以使用诊断设置来配置非计算资源的诊断日志。 用于资源控制的诊断设置具有以下功能：

* 指定要将诊断日志发送到何处。 例如，发送到 Azure 存储帐户、Azure 事件中心或 Monitor 日志。
* 指定要发送的日志类别。
* 指定要将每个日志类别保留在存储帐户中多长时间。
* 保留期为 0 天表示永久保留日志。 如果不需要永久保留，可将该值设置为 1 到 2,147,483,647 的任意天数。
* 如果设置了保留策略，但禁止将日志存储在存储帐户中，则保留策略无效。 例如，如果仅选择了“事件中心”或“Monitor 日志”选项，此可能会发生这种情况。
* 保留策略按天应用。 一天的结束时间可能出现在协调世界时 (UTC) 的午夜。 在一天结束时，会删除当天已超过保留策略期限的日志。 例如，如果保留策略的期限为一天，则在今天开始时，会删除前天的日志。

### <a name="enable-diagnostic-logs-via-the-azure-monitor-rest-api"></a>通过 Azure Monitor REST API 启用诊断日志

#### <a name="create-or-update-a-diagnostics-setting-in-the-monitor-rest-api"></a>在 Monitor REST API 中创建或更新诊断设置

##### <a name="request"></a>请求

```
PUT
https://management.chinacloudapi.cn/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>头文件

* 将 `{api-version}` 替换为 `2016-09-01`。
* 将 `{resource-id}` 替换为要编辑其诊断设置的资源的 ID。 有关详细信息，请参阅[使用资源组管理 Azure 资源](../azure-resource-manager/management/manage-resource-groups-portal.md)。
* 将 `Content-Type` 标头设置为 `application/json`。
* 将授权标头设置为从 Azure Active Directory (Azure AD) 获取的 JSON Web 令牌。 有关详细信息，请参阅[对请求进行身份验证](../active-directory/develop/authentication-vs-authorization.md)。

##### <a name="body"></a>正文

```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "metrics": [
        ],
        "logs": [
                {
                    "category": "PipelineRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "TriggerRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "ActivityRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                }
            ]
    },
    "location": ""
}
```

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| **storageAccountId** |String | 要将诊断日志发送到的存储帐户的资源 ID。 |
| **serviceBusRuleId** |String | 服务总线命名空间的服务总线规则 ID。你要在该服务总线命名空间中创建事件中心，以便流式传输诊断日志。 规则 ID 的格式为 `{service bus resource ID}/authorizationrules/{key name}`。|
| **workspaceId** | 复杂类型 | 指标时间粒度及其保留策略的数组。 此属性的值为空。 |
|**指标**| 要传递到被调用管道的管道运行的参数值| 将参数名映射为自变量值的 JSON 对象。 |
| **logs**| 复杂类型| 某个资源类型的诊断日志类别的名称。 若要获取资源的诊断日志类别列表，请先执行 GET 诊断设置操作。 |
| **category**| String| 日志类别及其保留策略的数组。 |
| **timeGrain** | String | 以 ISO 8601 持续时间格式捕获的指标的粒度。 该属性值必须为 `PT1M`（1 分钟）。 |
| **enabled**| 布尔 | 指定是否为此资源启用了该指标或日志类别的收集。 |
| **retentionPolicy**| 复杂类型| 描述指标或日志类别的保留策略。 此属性仅用于存储帐户。 |
|**days**| int| 指标或日志的保留天数。 如果该属性值为 0，则永久保留日志。 此属性仅用于存储帐户。 |

##### <a name="response"></a>响应

200 正常。


```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```

#### <a name="get-information-about-diagnostics-settings-in-the-monitor-rest-api"></a>在 Monitor REST API 中获取有关诊断设置的信息

##### <a name="request"></a>请求

```
GET
https://management.chinacloudapi.cn/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>头文件

* 将 `{api-version}` 替换为 `2016-09-01`。
* 将 `{resource-id}` 替换为要编辑其诊断设置的资源的 ID。 有关详细信息，请参阅[使用资源组管理 Azure 资源](../azure-resource-manager/management/manage-resource-groups-portal.md)。
* 将 `Content-Type` 标头设置为 `application/json`。
* 将授权标头设置为从 Azure AD 获取的 JSON Web 令牌。 有关详细信息，请参阅[对请求进行身份验证](../active-directory/develop/authentication-vs-authorization.md)。

##### <a name="response"></a>响应

200 正常。

```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}

```
有关详细信息，请参阅[诊断设置](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)。

## <a name="schema-of-logs-and-events"></a>日志和事件的架构

### <a name="monitor-schema"></a>监视架构

#### <a name="activity-run-log-attributes"></a>活动运行日志属性

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "activityRunId":"",
   "pipelineRunId":"",
   "resourceId":"",
   "category":"ActivityRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "activityName":"",
   "start":"",
   "end":"",
   "properties":
       {
          "Input": "{
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "BlobSink"
              }
           }",
          "Output": "{"dataRead":121,"dataWritten":121,"copyDuration":5,
               "throughput":0.0236328132,"errors":[]}",
          "Error": "{
              "errorCode": "null",
              "message": "null",
              "failureType": "null",
              "target": "CopyBlobtoBlob"
        }
    }
}
```

| 属性 | 类型 | 说明 | 示例 |
| --- | --- | --- | --- |
| **级别** |String | 诊断日志的级别。 对于活动运行日志，请将该属性值设置为 4。 | `4` |
| **correlationId** |String | 用于跟踪特定请求的唯一 ID。 | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | String | 事件的时间，以时间跨度表示，采用 UTC 格式 `YYYY-MM-DDTHH:MM:SS.00000Z`。 | `2017-06-28T21:00:27.3534352Z` |
|**activityRunId**| String| 活动运行的 ID。 | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|**pipelineRunId**| String| 管道运行的 ID。 | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**resourceId**| String | 与数据工厂资源关联的 ID。 | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**category**| String | 诊断日志的类别。 请将该属性值设置为 `ActivityRuns`。 | `ActivityRuns` |
|**level**| String | 诊断日志的级别。 请将该属性值设置为 `Informational`。 | `Informational` |
|**operationName**| String | 活动的名称及其状态。 如果活动是启动检测信号，则属性值为 `MyActivity -`。 如果活动是结束检测信号，则属性值为 `MyActivity - Succeeded`。 | `MyActivity - Succeeded` |
|**pipelineName**| String | 管道的名称。 | `MyPipeline` |
|**activityName**| String | 活动的名称。 | `MyActivity` |
|**start**| String | 活动运行的开始时间，以时间跨度表示，采用 UTC 格式。 | `2017-06-26T20:55:29.5007959Z`|
|**end**| String | 活动运行的结束时间，以时间跨度表示，采用 UTC 格式。 如果诊断日志显示活动已启动但尚未结束，则该属性值为 `1601-01-01T00:00:00Z`。 | `2017-06-26T20:55:29.5007959Z` |

#### <a name="pipeline-run-log-attributes"></a>管道运行日志属性

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "runId":"",
   "resourceId":"",
   "category":"PipelineRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "start":"",
   "end":"",
   "status":"",
   "properties":
    {
      "Parameters": {
        "<parameter1Name>": "<parameter1Value>"
      },
      "SystemParameters": {
        "ExecutionStart": "",
        "TriggerId": "",
        "SubscriptionId": ""
      }
    }
}
```

| 属性 | 类型 | 说明 | 示例 |
| --- | --- | --- | --- |
| **级别** |String | 诊断日志的级别。 对于活动运行日志，请将该属性值设置为 4。 | `4` |
| **correlationId** |String | 用于跟踪特定请求的唯一 ID。 | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | String | 事件的时间，以时间跨度表示，采用 UTC 格式 `YYYY-MM-DDTHH:MM:SS.00000Z`。 | `2017-06-28T21:00:27.3534352Z` |
|**runId**| String| 管道运行的 ID。 | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**resourceId**| String | 与数据工厂资源关联的 ID。 | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**category**| String | 诊断日志的类别。 请将该属性值设置为 `PipelineRuns`。 | `PipelineRuns` |
|**level**| String | 诊断日志的级别。 请将该属性值设置为 `Informational`。 | `Informational` |
|**operationName**| String | 管道的名称及其状态。 管道运行完成后，该属性值为 `Pipeline - Succeeded`。 | `MyPipeline - Succeeded`。 |
|**pipelineName**| String | 管道的名称。 | `MyPipeline` |
|**start**| String | 活动运行的开始时间，以时间跨度表示，采用 UTC 格式。 | `2017-06-26T20:55:29.5007959Z`。 |
|**end**| String | 活动运行的结束时间，以时间跨度表示，采用 UTC 格式。 如果诊断日志显示活动已启动但尚未结束，则该属性值为 `1601-01-01T00:00:00Z`。  | `2017-06-26T20:55:29.5007959Z` |
|**status**| String | 管道运行的最终状态。 可能的属性值为 `Succeeded` 和 `Failed`。 | `Succeeded`|

#### <a name="trigger-run-log-attributes"></a>触发器运行日志属性

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "triggerId":"",
   "resourceId":"",
   "category":"TriggerRuns",
   "level":"Informational",
   "operationName":"",
   "triggerName":"",
   "triggerType":"",
   "triggerEvent":"",
   "start":"",
   "status":"",
   "properties":
   {
      "Parameters": {
        "TriggerTime": "",
       "ScheduleTime": ""
      },
      "SystemParameters": {}
    }
}

```

| 属性 | 类型 | 说明 | 示例 |
| --- | --- | --- | --- |
| **级别** |String | 诊断日志的级别。 对于活动运行日志，请将该属性值设置为 4。 | `4` |
| **correlationId** |String | 用于跟踪特定请求的唯一 ID。 | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | String | 事件的时间，以时间跨度表示，采用 UTC 格式 `YYYY-MM-DDTHH:MM:SS.00000Z`。 | `2017-06-28T21:00:27.3534352Z` |
|**triggerId**| String| 触发器运行的 ID。 | `08587023010602533858661257311` |
|**resourceId**| String | 与数据工厂资源关联的 ID。 | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**category**| String | 诊断日志的类别。 请将该属性值设置为 `PipelineRuns`。 | `PipelineRuns` |
|**level**| String | 诊断日志的级别。 请将该属性值设置为 `Informational`。 | `Informational` |
|**operationName**| String | 触发器的名称及其最终状态（指示是否已成功激发该触发器）。 如果检测信号成功，则该属性值为 `MyTrigger - Succeeded`。 | `MyTrigger - Succeeded` |
|**triggerName**| String | 触发器的名称。 | `MyTrigger` |
|**triggerType**| String | 触发器的类型。 可能的属性值为 `Manual Trigger` 和 `Schedule Trigger`。 | `ScheduleTrigger` |
|**triggerEvent**| String | 触发器的事件。 | `ScheduleTime - 2017-07-06T01:50:25Z` |
|**start**| String | 激发触发器的开始时间，以时间跨度表示，采用 UTC 格式。 | `2017-06-26T20:55:29.5007959Z`|
|**status**| String | 最终状态（指示是否已成功激发该触发器）。 可能的属性值为 `Succeeded` 和 `Failed`。 | `Succeeded`|

### <a name="log-analytics-schema"></a>Log Analytics 架构

Log Analytics 从 Monitor 继承架构，但存在以下例外情况：

* 每个列名中的第一个字母会大写。 例如，Monitor 中的列名“correlationId”在 Log Analytics 中为“CorrelationId”。
* 没有“Level”列。
* 动态“properties”列保留为以下动态 JSON Blob 类型。

    | Azure Monitor 列 | Log Analytics 列 | 类型 |
    | --- | --- | --- |
    | $.properties.UserProperties | UserProperties | 动态 |
    | $.properties.Annotations | 批注 | 动态 |
    | $.properties.Input | 输入 | 动态 |
    | $.properties.Output | 输出 | 动态 |
    | $.properties.Error.errorCode | ErrorCode | int |
    | $.properties.Error.message | ErrorMessage | string |
    | $.properties.Error | 错误 | 动态 |
    | $.properties.Predecessors | Predecessors | 动态 |
    | $.properties.Parameters | parameters | 动态 |
    | $.properties.SystemParameters | SystemParameters | 动态 |
    | $.properties.Tags | Tags | 动态 |


## <a name="next-steps"></a>后续步骤
[以编程方式监视和管理管道](monitor-programmatically.md)
