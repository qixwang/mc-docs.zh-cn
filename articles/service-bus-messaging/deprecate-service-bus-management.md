---
title: Azure 消息传送服务 - 从服务管理器迁移到资源管理器
description: 本文提供了从弃用的 Azure 服务管理器 REST API 和 PowerShell cmdlet 到资源管理器 REST API 和 PowerShell cmdlet 的映射。
services: service-bus-messaging, event-hubs, event-grid
documentationcenter: na
author: spelluru
editor: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/29/2020
ms.author: v-tawe
origin.date: 01/22/2020
ms.openlocfilehash: f99b93c14e829bb69b0c2f2ee682a04ec8e78e1d
ms.sourcegitcommit: 1fbdefdace8a1d3412900c6c3f89678d8a9b29bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82887003"
---
# <a name="deprecation-of-azure-service-manager-support-for-azure-service-bus-relay-and-event-hubs"></a>弃用了 Azure 服务管理器对 Azure 服务总线、中继和事件中心的支持

资源管理器是我们的下一代云基础结构堆栈，正在完全取代“经典”Azure 服务管理模型（经典部署模型）。 因此，我们将于 2021 年 11 月 1 日停用经典部署模型 REST API 以及对服务总线、中继和事件中心的支持。 我们首先在 [Microsoft 技术社区公告](https://techcommunity.microsoft.com/t5/Service-Bus-blog/Deprecating-Service-Management-support-for-Azure-Service-Bus/ba-p/370909)上宣告了该停用决定，但最近又决定将弃用期在原始公告时间的基础上再延长两年。 这些 API 的 URI 中包含 `management.core.chinacloudapi.cn`，这是为了方便识别。 请参阅下表，了解一系列已弃用的 API 及其 Azure 资源管理器 API 版本，后者是你现在应该使用的。

若继续使用服务总线、中继和事件中心，请在 2021 年 10 月 31 日之前迁移到资源管理器。 我们鼓励所有仍在使用旧 API 的客户尽快转换版本，以便利用资源管理器带来的其他好处，这包括资源分组、标记、简化的部署和管理过程，以及使用基于角色的访问控制 (RBAC) 进行精细的访问控制。

若要详细了解 Azure 资源管理器与 Azure 服务管理器的对比情况，请查看 [TechNet 博客](https://blogs.technet.microsoft.com/meamcs/2016/12/22/difference-between-azure-service-manager-and-azure-resource-manager/)。

若要详细了解适用于 Azure 服务总线、中继和事件中心的服务管理器 API 和资源管理器 API，请参阅我们的 REST API 文档：

- [Azure 服务总线](https://docs.microsoft.com/rest/api/servicebus/)
- [Azure 事件中心](https://docs.microsoft.com/rest/api/eventhub/)
- [Azure 中继](https://docs.microsoft.com/rest/api/relay/)

## <a name="service-manager-rest-api---resource-manager-rest-api"></a>服务管理器 REST API - 资源管理器 REST API

| 服务管理器 API（已弃用） | 资源管理器 - 服务总线 API | 资源管理器 - 事件中心 API | 资源管理器 - 中继 API |
| --------------- | ----------------- | ----------------- | ----------------- | 
| **Namespaces-GetNamespaceAsync** <br/>[服务总线获取命名空间](https://docs.microsoft.com/rest/api/servicebus/get-namespace)<br/>[事件中心获取命名空间](https://docs.microsoft.com/rest/api/eventhub/get-event-hub)<br/>[中继获取命名空间](https://docs.microsoft.com/rest/api/servicebus/get-relays)<br/> ```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [get](https://docs.microsoft.com/rest/api/servicebus/namespaces/get) | get | [get](https://docs.microsoft.com/rest/api/relay/namespaces/get) |
| **ConnectionDetails-GetConnectionDetails**<br/>服务总线/事件中心/中继 GetConnectionDetals<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/ConnectionDetails``` | [listkeys](https://docs.microsoft.com/rest/api/servicebus/namespaces/listkeys) | listkeys | [listkeys](https://docs.microsoft.com/rest/api/relay/namespaces/listkeys) |
| **Topics-GetTopicsAsync**<br/>服务总线<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/topics? $skip={skip}&$top={top}``` | [list](https://docs.microsoft.com/rest/api/servicebus/topics/listbynamespace) | &nbsp; | &nbsp; | 
| **Queues-GetQueueAsync** <br/>服务总线<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/queues/{queueName}``` | [get](https://docs.microsoft.com/rest/api/servicebus/queues/get) | &nbsp; | &nbsp; | 
| **Relays-GetRelaysAsync**<br/>[获取中继](https://docs.microsoft.com/rest/api/servicebus/get-relays)<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/relays? $skip={skip}&$top={top}```| &nbsp; | &nbsp; | [list](https://docs.microsoft.com/rest/api/relay/wcfrelays/listbynamespace) | 
| **NamespaceAuthorizationRules-GetNamespaceAuthorizationRuleAsync**<br/>服务总线/事件中心/中继 GetNamespaceAuthRule<br/>```GET https://managementcore.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/authorizationrules?``` | [getauthorizationrule](https://docs.microsoft.com/rest/api/servicebus/namespaces/getauthorizationrule) | getauthorizationrule | [getauthorizationrule](https://docs.microsoft.com/rest/api/relay/namespaces/getauthorizationrule) |
| **Namespaces-DeleteNamespaceAsync**<br/>[服务总线删除命名空间](https://docs.microsoft.com/rest/api/servicebus/delete-namespace)<br/>[事件中心删除命名空间](https://docs.microsoft.com/rest/api/eventhub/delete-event-hub)<br/>[中继删除命名空间](https://docs.microsoft.com/rest/api/servicebus/delete-namespace)<br/> ```DELETE    https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [delete](https://docs.microsoft.com/rest/api/servicebus/namespaces/delete) | 删除 | [delete](https://docs.microsoft.com/rest/api/relay/namespaces/delete) | 
| **MessagingSKUPlan-GetPlanAsync**<br/>服务总线/事件中心/中继获取命名空间<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/MessagingPlan``` | [get](https://docs.microsoft.com/rest/api/servicebus/namespaces/get) | get | [get](https://docs.microsoft.com/rest/api/relay/namespaces/get) |
| **MessagingSKUPlan-UpdatePlanAsync**<br/>服务总线/事件中心/中继获取命名空间<br/>```PUT https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/MessagingPlan``` | [createorupdate](https://docs.microsoft.com/rest/api/servicebus/namespaces/createorupdate) | createorupdate | [createorupdate](https://docs.microsoft.com/rest/api/relay/namespaces/createorupdate) |
| **NamespaceAuthorizationRules-UpdateNamespaceAuthorizationRuleAsync**<br/>服务总线/事件中心/中继获取命名空间<br/>```PUT https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` | [createorupdate](https://docs.microsoft.com/rest/api/servicebus/namespaces/createorupdate) | createorupdateauthorizationrule | [createorupdateauthorizationrule](https://docs.microsoft.com/rest/api/relay/namespaces/createorupdateauthorizationrule) | 
| **NamespaceAuthorizationRules-CreateNamespaceAuthorizationRuleAsync**<br/> 
服务总线/事件中心/中继<br/>```PUT https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` |[createorupdate](https://docs.microsoft.com/rest/api/servicebus/namespaces/createorupdate) | createorupdateauthorizationrule | [createorupdateauthorizationrule](https://docs.microsoft.com/rest/api/relay/namespaces/createorupdateauthorizationrule) |
| **NamespaceProperties-GetNamespacePropertiesAsync**<br/>[服务总线获取命名空间](https://docs.microsoft.com/rest/api/servicebus/get-namespace)<br/>[事件中心获取命名空间](https://docs.microsoft.com/rest/api/eventhub/get-event-hub)<br/>[中继获取命名空间](https://docs.microsoft.com/rest/api/servicebus/get-relays)<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [get](https://docs.microsoft.com/rest/api/servicebus/namespaces/get) | get | [get](https://docs.microsoft.com/rest/api/relay/namespaces/get) |
| **RegionCodes-GetRegionCodesAsync**<br/>服务总线/事件中心/中继获取命名空间<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [listbysku](https://docs.microsoft.com/rest/api/servicebus/regions/listbysku) | listbysku | &nbsp; | 
| **NamespaceProperties-UpdateNamespacePropertyAsync**<br/>服务总线/事件中心/中继<br/>```GET    https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Regions/``` | [createorupdate](https://docs.microsoft.com/rest/api/servicebus/namespaces/createorupdate) | createorupdate | [createorupdate](https://docs.microsoft.com/rest/api/relay/namespaces/createorupdate) |
| **EventHubsCrud-ListEventHubsAsync**<br/>[列出事件中心](https://docs.microsoft.com/rest/api/eventhub/list-event-hubs)<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/eventhubs?$skip={skip}&$top={top}``` | &nbsp; | [list](https://docs.microsoft.com/rest/api/servicebus/eventhubs/listbynamespace) | &nbsp; | 
| **EventHubsCrud-GetEventHubAsync**<br/>[获取事件中心](https://docs.microsoft.com/rest/api/eventhub/get-event-hub)<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/eventhubs/{eventHubPath}``` | &nbsp; | [get](https://docs.microsoft.com/rest/api/eventhub/get-event-hub) | &nbsp; | 
| **NamespaceAuthorizationRules-DeleteNamespaceAuthorizationRuleAsync**<br/>服务总线/事件中心/中继<br/>```DELETE https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` | [deleteauthorizationrule](https://docs.microsoft.com/rest/api/servicebus/namespaces/deleteauthorizationrule) | deleteauthorizationrule | [deleteauthorizationrule](https://docs.microsoft.com/rest/api/relay/namespaces/deleteauthorizationrule) |
| **NamespaceAuthorizationRules-GetNamespaceAuthorizationRulesAsync**<br/>服务总线/事件中心/中继<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules``` | [listauthorizationrules](https://docs.microsoft.com/rest/api/servicebus/namespaces/listauthorizationrules) | listauthorizationrules | [listauthorizationrules](https://docs.microsoft.com/rest/api/relay/namespaces/listauthorizationrules) |
| **NamespaceAvailability-IsNamespaceAvailable**<br/>[服务总线命名空间可用性](https://docs.microsoft.com/rest/api/servicebus/check-namespace-availability)<br/>```GET   https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/CheckNamespaceAvailability/?namespace=<namespaceValue>``` | [checknameavailability](https://docs.microsoft.com/rest/api/servicebus/namespaces/checknameavailability) | checknameavailability | [checknameavailability](https://docs.microsoft.com/rest/api/relay/namespaces/checknameavailability) |
| **Namespaces-CreateOrUpdateNamespaceAsync**<br/>服务总线/事件中心/中继<br/>```PUT https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [createorupdate](https://docs.microsoft.com/rest/api/servicebus/namespaces/createorupdate) | createorupdate | [createorupdate](https://docs.microsoft.com/rest/api/relay/namespaces/createorupdate) | 
| **Topics-GetTopicAsync**<br/>```GET https://management.core.chinacloudapi.cn/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/topics/{topicPath}``` | [get](https://docs.microsoft.com/rest/api/servicebus/topics/get) | &nbsp; | &nbsp; |

## <a name="service-manager-powershell---resource-manager-powershell"></a>服务管理器 PowerShell - 资源管理器 PowerShell
| 服务管理器 PowerShell 命令（已弃用） | 新资源管理器命令 | 更新的资源管理器命令 |
| ----- | ----- | ----- | 
| [Get-AzureSBAuthorizationRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbauthorizationrule?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusAuthorizationRule](https://docs.microsoft.com/powershell/module/azurerm.servicebus/get-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [Get-AzServiceBusAuthorizationRule](https://docs.microsoft.com/powershell/module/az.servicebus/get-azservicebusauthorizationrule?view=azps-1.6.0) |
| [Get-AzureSBLocation](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresblocation?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusGeoDRConfiguration](https://docs.microsoft.com/powershell/module/azurerm.servicebus/get-azurermservicebusgeodrconfiguration?view=azurermps-6.13.0) | [Get-AzServiceBusGeoDRConfiguration](https://docs.microsoft.com/powershell/module/az.servicebus/get-azservicebusgeodrconfiguration?view=azps-1.6.0) |
| [Get-AzureSBNamespace](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbnamespace?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusNamespace](https://docs.microsoft.com/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace?view=azurermps-6.13.0) | [Get-AzServiceBusNamespace](https://docs.microsoft.com/powershell/module/az.servicebus/get-azservicebusnamespace?view=azps-1.6.0) |
| [New-AzureSBAuthorizationRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azuresbauthorizationrule?view=azuresmps-4.0.0) | [New-AzureRmServiceBusAuthorizationRule](https://docs.microsoft.com/powershell/module/azurerm.servicebus/new-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [New-AzServiceBusAuthorizationRule](https://docs.microsoft.com/powershell/module/az.servicebus/new-azservicebusauthorizationrule?view=azps-1.6.0) |
| [New-AzureSBNamespace](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azuresbnamespace?view=azuresmps-4.0.0) | [New-AzureRmServiceBusNamespace](https://docs.microsoft.com/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace?view=azurermps-6.13.0) | [New-AzServiceBusNamespace](https://docs.microsoft.com/powershell/module/az.servicebus/new-azservicebusnamespace?view=azps-1.6.0) |
| [Remove-AzureRmRelayAuthorizationRule](https://docs.microsoft.com/powershell/module/azurerm.relay/remove-azurermrelayauthorizationrule?view=azurermps-6.13.0) | [Remove-AzureRmEventHubAuthorizationRule](https://docs.microsoft.com/powershell/module/azurerm.eventhub/remove-azurermeventhubauthorizationrule?view=azurermps-6.13.0) | [Remove-AzServiceBusAuthorizationRule](https://docs.microsoft.com/powershell/module/az.servicebus/remove-azservicebusauthorizationrule?view=azps-1.6.0) |
| [Remove-AzureSBNamespace](https://docs.microsoft.com/powershell/module/servicemanagement/azure/remove-azuresbnamespace?view=azuresmps-4.0.0) | [Remove-AzureRmServiceBusNamespace](https://docs.microsoft.com/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace?view=azurermps-6.13.0) | [Remove-AzServiceBusNamespace](https://docs.microsoft.com/powershell/module/az.servicebus/remove-azservicebusnamespace?view=azps-1.6.0) |
| [Set-AzureSBAuthorizationRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azuresbauthorizationrule?view=azuresmps-4.0.0) | [Set-AzureRmServiceBusAuthorizationRule](https://docs.microsoft.com/powershell/module/azurerm.servicebus/set-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [Set-AzServiceBusAuthorizationRule](https://docs.microsoft.com/powershell/module/az.servicebus/set-azservicebusauthorizationrule?view=azps-1.6.0) |

## <a name="next-steps"></a>后续步骤
请参阅以下文档： 

- 最新 REST API 文档
    - [Azure 服务总线](https://docs.microsoft.com/rest/api/servicebus/)
    - [Azure 事件中心](https://docs.microsoft.com/rest/api/eventhub/)
    - [Azure 中继](https://docs.microsoft.com/rest/api/relay/)
- 最新 PowerShell 文档
    - [Azure 服务总线](https://docs.microsoft.com/powershell/module/azurerm.servicebus/?view=azurermps-6.13.0#service_bus)
    - [Azure 事件中心](https://docs.microsoft.com/powershell/module/azurerm.eventhub/?view=azurermps-6.13.0#event_hub)
    - [Azure 事件网格](https://docs.microsoft.com/powershell/module/azurerm.eventgrid/?view=azurermps-6.13.0#event_grid)
