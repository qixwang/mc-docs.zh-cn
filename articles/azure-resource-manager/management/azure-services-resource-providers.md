---
title: 按 Azure 服务列出的资源提供程序
description: 列出 Azure 资源管理器的所有资源提供程序命名空间，并显示该命名空间的 Azure 服务。
ms.topic: conceptual
origin.date: 11/11/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: b82147925a8c8cc4a6899bcc578dfaf03a5de145
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631207"
---
<!--Verify sucessfully-->
# <a name="resource-providers-for-azure-services"></a>Azure 服务的资源提供程序

本文介绍资源提供程序命名空间如何映射到 Azure 服务。

## <a name="match-resource-provider-to-service"></a>将资源提供程序匹配到服务

| 资源提供程序命名空间 | Azure 服务 |
| --------------------------- | ------------- |
| Microsoft.AAD | [Azure Active Directory 域服务 |
| Microsoft.Advisor | [Azure 顾问](/advisor/index/) |
| Microsoft.AlertsManagement | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.AnalysisServices | [Azure Analysis Services](/analysis-services/) |
| Microsoft.ApiManagement | [API 管理](../../api-management/index.yml) |
| Microsoft.Authorization | [Azure 资源管理器](../index.yml) |
| Microsoft.Automation | [自动化](../../automation/index.yml) |
| Microsoft.AzureActiveDirectory | [Azure Active Directory B2C](../../active-directory-b2c/index.yml) |
| Microsoft.AzureStack | core |
| Microsoft.Batch | [批处理](../../batch/index.yml) |
| Microsoft.Cache | [用于 Redis 的 Azure 缓存](/azure-cache-for-redis/) |
| Microsoft.Cdn | [内容分发网络](/cdn/) |
| Microsoft.ClassicCompute | 经典部署模型虚拟机 |
| Microsoft.ClassicInfrastructureMigrate | 经典部署模型迁移 |
| Microsoft.ClassicNetwork | 经典部署模型虚拟网络 |
| Microsoft.ClassicStorage | 经典部署模型存储 |
| Microsoft.ClassicSubscription | 经典部署模型 |
| Microsoft.CognitiveServices | [认知服务](/cognitive-services/) |
| Microsoft.Compute | [虚拟机](/virtual-machines/)<br />[虚拟机规模集](/virtual-machine-scale-sets/) |
| Microsoft.ContainerInstance | 容器实例 |
| Microsoft.ContainerRegistry | [容器注册表](/container-registry/) |
| Microsoft.ContainerService | [Azure Kubernetes 服务 (AKS)](/aks/) |
| Microsoft.DataFactory | [数据工厂](/data-factory/) |
| Microsoft.DataMigration | [Azure 数据库迁移服务](/dms/) |
| Microsoft.DBforMariaDB | [Azure Database for MariaDB](/mariadb/) |
| Microsoft.DBforMySQL | [Azure Database for MySQL](/mysql/) |
| Microsoft.DBforPostgreSQL | [Azure Database for PostgreSQL](/postgresql/) |
| Microsoft.Devices | [IoT 中心](/iot-hub/)<br />[IoT 中心设备预配服务](/iot-dps/) |
| Microsoft.DocumentDB | [Azure Cosmos DB](../../cosmos-db/index.yml) |
| Microsoft.EventGrid | [事件网格](/event-grid/) |
| Microsoft.EventHub | [事件中心](../../event-hubs/index.yml) |
| Microsoft.Features | [Azure 资源管理器](../index.yml) |
| Microsoft.GuestConfiguration | [Azure Policy](../../governance/policy/index.yml) |
| Microsoft.HDInsight | [HDInsight](../../hdinsight/index.yml) |
| Microsoft.ImportExport | [Azure 导入/导出](../../storage/common/storage-import-export-service.md) |
| microsoft.insights | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.IoTCentral | IoT Central |
| Microsoft.KeyVault | [密钥保管库](../../key-vault/index.yml) |
| Microsoft.Kusto | [Azure 数据资源管理器](../../data-explorer/index.yml) |
| Microsoft.Logic | [逻辑应用](../../logic-apps/index.yml) |
| Microsoft.ManagedIdentity | [Azure 资源的托管标识](../../active-directory/managed-identities-azure-resources/index.yml) |
| Microsoft.Management | [管理组](/governance/management-groups/) |
| Microsoft.Media | [媒体服务](../../media-services/index.yml) |
| Microsoft.Network | [虚拟网络](../../virtual-network/index.yml)<br />[负载均衡器](../../load-balancer/index.yml)<br />[应用程序网关](../../application-gateway/index.yml)<br />[Azure DNS](../../dns/index.yml)<br />[ExpressRoute](../../expressroute/index.yml)<br />[VPN 网关](../../vpn-gateway/index.yml)<br />[流量管理器](../../traffic-manager/index.yml)<br />[网络观察程序](../../network-watcher/index.yml)<br />[Azure 防火墙](../../firewall/index.yml) |
| Microsoft.NotificationHubs | [通知中心](../../notification-hubs/index.yml) |
| Microsoft.OperationalInsights | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.OperationsManagement | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.PolicyInsights | [Azure Policy](../../governance/policy/index.yml) |
| Microsoft.Portal | [Azure 门户](/azure-portal/) |
| Microsoft.PowerBI | [Power BI](https://docs.microsoft.com/power-bi/power-bi-overview) |
| Microsoft.PowerBIDedicated | [Power BI Embedded](/power-bi-embedded/) |
| Microsoft.RecoveryServices | [站点恢复](../../site-recovery/index.yml) |
| Microsoft.Relay | [Azure 中继](../../service-bus-relay/relay-what-is-it.md) |
| Microsoft.ResourceGraph | [Azure Resource Graph](/governance/resource-graph/) |
| Microsoft.ResourceHealth | core |
| Microsoft.Resources | [Azure Resource Manager](../index.yml) |
| Microsoft.Scheduler | [计划程序](/scheduler/) |
| Microsoft.Search | [Azure 搜索](../../search/index.yml) |
| Microsoft.Security | [安全中心](../../security-center/index.yml) |
| Microsoft.ServiceBus | [服务总线](/service-bus/) |
| Microsoft.ServiceFabric | [Service Fabric](../../service-fabric/index.yml) |
| Microsoft.SignalRService | [Azure SignalR 服务](../../azure-signalr/index.yml) |
| Microsoft.SiteRecovery | [站点恢复](../../site-recovery/index.yml) |
| Microsoft.Solutions | Azure 托管应用程序 |
| Microsoft.Sql | [Azure SQL 数据库](../../sql-database/index.yml)<br />[SQL 数据仓库](/sql-data-warehouse/) |
| Microsoft.Storage | [存储](../../storage/index.yml) |
| Microsoft.StreamAnalytics | [流分析](../../stream-analytics/index.yml) |
| Microsoft.TimeSeriesInsights | [时序见解](../../time-series-insights/index.yml) |
| Microsoft.Web | [应用服务](../../app-service/index.yml)<br />[函数](../../azure-functions/index.yml) |

## <a name="next-steps"></a>后续步骤

有关资源提供程序的详细信息，请参阅 [Azure 资源提供程序和类型](resource-providers-and-types.md)

<!-- Update_Description: update meta properties, wording update, update link -->