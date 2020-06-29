---
title: 按 Azure 服务列出的资源提供程序
description: 列出 Azure 资源管理器的所有资源提供程序命名空间，并显示该命名空间的 Azure 服务。
ms.topic: conceptual
origin.date: 06/05/2020
ms.date: 06/22/2020
ms.author: v-yeche
ms.openlocfilehash: 7ca22a2881b1ca94bb62db2eee6aec72262a3ca4
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85098682"
---
<!--Verify sucessfully on 2020/06/18 by harris-->
# <a name="resource-providers-for-azure-services"></a>Azure 服务的资源提供程序

本文介绍资源提供程序命名空间如何映射到 Azure 服务。

## <a name="match-resource-provider-to-service"></a>将资源提供程序匹配到服务

| 资源提供程序命名空间 | Azure 服务 |
| --------------------------- | ------------- |
| Microsoft.AAD | Azure Active Directory 域服务 |
| Microsoft.Addons | core |
| Microsoft.ADHybridHealthService<sup>1</sup> | [Azure Active Directory](/active-directory/) |
| Microsoft.Advisor | [Azure 顾问](/advisor/) |
| Microsoft.AlertsManagement | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.AnalysisServices | [Azure Analysis Services](/analysis-services/) |
| Microsoft.ApiManagement | [API 管理](../../api-management/index.yml) |
| Microsoft.Authorization<sup>1</sup> | [Azure 资源管理器](../index.yml) |
| Microsoft.Automation | [自动化](../../automation/index.yml) |
| Microsoft.AzureActiveDirectory | [Azure Active Directory B2C](../../active-directory-b2c/index.yml) |
| Microsoft.AzureData | SQL Server 注册表 |
| Microsoft.AzureStack | core |
| Microsoft.Batch | [批处理](../../batch/index.yml) |
| Microsoft.Billing<sup>1</sup> | [成本管理和计费](/billing/) |
| Microsoft.Blueprint | Azure 蓝图 |
| Microsoft.Cache | [用于 Redis 的 Azure 缓存](/azure-cache-for-redis/) |
| Microsoft.Capacity | core |
| Microsoft.Cdn | [内容分发网络](/cdn/) |
| Microsoft.ChangeAnalysis | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.ClassicCompute | 经典部署模型虚拟机 |
| Microsoft.ClassicInfrastructureMigrate | 经典部署模型迁移 |
| Microsoft.ClassicNetwork | 经典部署模型虚拟网络 |
| Microsoft.ClassicStorage | 经典部署模型存储 |
| Microsoft.ClassicSubscription<sup>1</sup> | 经典部署模型 |
| Microsoft.CognitiveServices | [认知服务](/cognitive-services/) |
| Microsoft.Commerce<sup>1</sup> | core |
| Microsoft.Compute | [虚拟机](/virtual-machines/)<br />[虚拟机规模集](/virtual-machine-scale-sets/) |
| Microsoft.ContainerInstance | [容器实例](/container-instances/) |
| Microsoft.ContainerRegistry | [容器注册表](/container-registry/) |
| Microsoft.ContainerService | [Azure Kubernetes 服务 (AKS)](/aks/) |
| Microsoft.DataFactory | [数据工厂](/data-factory/) |
| Microsoft.DataMigration | [Azure 数据库迁移服务](/dms/) |
| Microsoft.DBforMariaDB | [Azure Database for MariaDB](/mariadb/) |
| Microsoft.DBforMySQL | [Azure Database for MySQL](/mysql/) |
| Microsoft.DBforPostgreSQL | [Azure Database for PostgreSQL](/postgresql/) |
| Microsoft.Devices | [Azure IoT 中心](/iot-hub/)<br />[Azure IoT 中心设备预配服务](/iot-dps/) |
| Microsoft.DocumentDB | [Azure Cosmos DB](../../cosmos-db/index.yml) |
| Microsoft.DomainRegistration | [应用服务](/app-service/) |
| Microsoft.EnterpriseKnowledgeGraph | 企业知识图 |
| Microsoft.EventGrid | [事件网格](/event-grid/) |
| Microsoft.EventHub | [事件中心](../../event-hubs/index.yml) |
| Microsoft.Features<sup>1</sup> | [Azure 资源管理器](../index.yml) |
| Microsoft.GuestConfiguration | [Azure Policy](../../governance/policy/index.yml) |
| Microsoft.HDInsight | [HDInsight](../../hdinsight/index.yml) |
| Microsoft.ImportExport | [Azure 导入/导出](../../storage/common/storage-import-export-service.md) |
| microsoft.insights | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.IoTCentral | [Azure IoT Central](/iot-central/) |
| Microsoft.KeyVault | [密钥保管库](../../key-vault/index.yml) |
| Microsoft.Kubernetes | [Azure Kubernetes 服务 (AKS)](/aks/) |
| Microsoft.Kusto | [Azure 数据资源管理器](/data-explorer/) |
| Microsoft.Logic | [逻辑应用](../../logic-apps/index.yml) |
| Microsoft.MachineLearningServices | [Azure 机器学习](../../machine-learning/index.yml) |
| Microsoft.Maintenance | [Azure 维护](../../virtual-machines/maintenance-control-cli.md) |
| Microsoft.ManagedIdentity | [Azure 资源的托管标识](../../active-directory/managed-identities-azure-resources/index.yml) |
| Microsoft.Management | [管理组](/governance/management-groups/) |
| Microsoft.Media | [媒体服务](../../media-services/index.yml) |
| Microsoft.Network | [应用程序网关](../../application-gateway/index.yml)<br />[Azure DNS](../../dns/index.yml)<br />[Azure ExpressRoute](../../expressroute/index.yml)<br />[Azure 防火墙](../../firewall/index.yml)<br />[负载均衡器](../../load-balancer/index.yml)<br />[网络观察程序](../../network-watcher/index.yml)<br />[流量管理器](../../traffic-manager/index.yml)<br />[虚拟网络](../../virtual-network/index.yml)<br />[虚拟 WAN](../../virtual-wan/index.yml)<br />[VPN 网关](../../vpn-gateway/index.yml)<br /> |
| Microsoft.NotificationHubs | [通知中心](../../notification-hubs/index.yml) |
| Microsoft.OperationalInsights | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.OperationsManagement | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.PolicyInsights | [Azure Policy](../../governance/policy/index.yml) |
| Microsoft.Portal<sup>1</sup> | [Azure 门户](/azure-portal/) |
| Microsoft.PowerBI | [Power BI](https://docs.microsoft.com/power-bi/power-bi-overview) |
| Microsoft.PowerBIDedicated | [Power BI Embedded](/power-bi-embedded/) |
| Microsoft.RecoveryServices | [Azure Site Recovery](../../site-recovery/index.yml) |
| Microsoft.Relay | [Azure 中继](../../service-bus-relay/relay-what-is-it.md) |
| Microsoft.ResourceGraph<sup>1</sup> | [Azure Resource Graph](/governance/resource-graph/) |
| Microsoft.ResourceHealth | [Azure 服务运行状况](../../service-health/index.yml) |
| Microsoft.Resources<sup>1</sup> | [Azure Resource Manager](../index.yml) |
| Microsoft.SaaS | core |
| Microsoft.Scheduler | [计划程序](/scheduler/) |
| Microsoft.Search | [Azure 认知搜索](../../search/index.yml) |
| Microsoft.Security | [安全中心](../../security-center/index.yml) |
| Microsoft.ServiceBus | [服务总线](/service-bus/) |
| Microsoft.ServiceFabric | [Service Fabric](../../service-fabric/index.yml) |
| Microsoft.SignalRService | [Azure SignalR 服务](../../azure-signalr/index.yml) |
| Microsoft.Solutions | [Azure 托管应用程序](../managed-applications/index.yml) |
| Microsoft.Sql | [Azure SQL 数据库](../../sql-database/index.yml)<br />[Azure Synapse Analytics](/sql-data-warehouse/) |
| Microsoft.Storage | [存储](../../storage/index.yml) |
| Microsoft.StreamAnalytics | [Azure 流分析](../../stream-analytics/index.yml) |
| Microsoft.Synapse | [Azure Synapse Analytics](/sql-data-warehouse/) |
| Microsoft.TimeSeriesInsights | [Azure 时序见解](../../time-series-insights/index.yml) |
| Microsoft.Web | [应用服务](../../app-service/index.yml)<br />[Azure Functions](../../azure-functions/index.yml) |
| Microsoft.WorkloadMonitor<sup>1</sup> | [Azure Monitor](/azure-monitor/) |

<sup>1</sup> 默认已注册

## <a name="next-steps"></a>后续步骤

有关资源提供程序的详细信息（包括如何注册资源提供程序），请参阅 [Azure 资源提供程序和类型](resource-providers-and-types.md)

<!-- Update_Description: update meta properties, wording update, update link -->