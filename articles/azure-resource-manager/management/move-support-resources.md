---
title: 资源类型支持的移动操作
description: 列出可移到新资源组或订阅的 Azure 资源类型。
ms.topic: conceptual
origin.date: 01/02/2020
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: f69828c7c6d054dfb08a268f9e165c2e07cc2972
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79293155"
---
# <a name="move-operation-support-for-resources"></a>支持移动操作的资源
本文列出某个 Azure 资源类型是否支持移动操作。 它还提供了有关移动资源时要考虑的特殊条件的信息。

跳转到资源提供程序命名空间：
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Web](#microsoftweb)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | domainservices | 否 | 否 |
> | domainservices/replicasets | 否 | 否 |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | tenants | 否 | 否 |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | actionrules | 是 | 是 |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | servers | 是 | 是 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 服务 | 是 | 是 |

<!--Not Available on ## Microsoft.AppConfiguration-->

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | apiapps | 否 | 否 |
> | appidentities | 否 | 否 |
> | gateways | 否 | 否 |

> [!IMPORTANT]
> 请参阅[应用服务移动指南](./move-limitations/app-service-move-limitations.md)。

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | policyassignments | 否 | 否 |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | automationaccounts | 是 | 是 |
> | automationaccounts/configurations | 是 | 是 |
> | automationaccounts/runbooks | 是 | 是 |

> [!IMPORTANT]
> Runbook 必须与自动化帐户存在于同一资源组中。

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | 是 | 是 |

<!-- Not Available on ## Microsoft.AzureData-->

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | registrations | 是 | 是 |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | batchaccounts | 是 | 是 |

<!-- Not Available on ## ## Microsoft.BatchAI-->
<!-- Not Available on ## Microsoft.BingMaps-->
<!-- Not Available on ## Microsoft.BizTalkServices-->
<!-- Not Available on ## Microsoft.Blockchain-->
<!-- Not Available on ## Microsoft.Blueprint-->
<!-- Not Available on ## Microsoft.BotService-->

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | redis | 是 | 是 |

> [!IMPORTANT]
> 如果 Azure Redis 缓存实例配置了虚拟网络，则无法将该实例移到其他订阅。 请参阅[网络移动限制](./move-limitations/networking-move-limitations.md)。

## <a name="microsoftcdn"></a>Microsoft.Cdn

<!--MOONCAKE CUSTOMIZATION: PROFILES AND PROFILES / ENDPOOINTS IS NO FOR SUBSCRIPTION-->
<!--UPDATE CAREFULLY, CORRECT BY EMAIL-->

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallpolicies | 否 | 否 |
> | 配置文件 | 是 | 否 |
> | profiles/endpoints | 是 | 否 |

> [!NOTE]
> 目前，Azure 中国不支持有关跨订阅移动 CDN 资源的自助服务。
> 当尝试在 Azure 中国中跨订阅移动 CDN 配置文件或配置文件/终结点时，请联系 [Azure 支持](https://support.azure.cn/support/contact/)或在 [Azure 支持网站](https://support.azure.cn/support/support-azure/)上提交请求以获得帮助。
> 

<!--UPDATE CAREFULLY, CORRECT BY EMAIL-->
<!--MOONCAKE CUSTOMIZATION: PROFILES AND PROFILES / ENDPOOINTS IS NO FOR SUBSCRIPTION-->

<!-- Not Available on ## Microsoft.CertificateRegistration-->

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | domainnames | 是 | 否 |
> | virtualmachines | 是 | 否 |

> [!IMPORTANT]
> 请参阅[经典部署移动指南](./move-limitations/classic-model-move-limitations.md)。 可以使用特定于该方案的操作跨订阅移动经典部署资源。

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | networksecuritygroups | 否 | 否 |
> | reservedips | 否 | 否 |
> | virtualnetworks | 否 | 否 |

> [!IMPORTANT]
> 请参阅[经典部署移动指南](./move-limitations/classic-model-move-limitations.md)。 可以使用特定于该方案的操作跨订阅移动经典部署资源。

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | storageaccounts | 是 | 否 |

> [!IMPORTANT]
> 请参阅[经典部署移动指南](./move-limitations/classic-model-move-limitations.md)。 可以使用特定于该方案的操作跨订阅移动经典部署资源。

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 是 | 是 |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | availabilitysets | 是 | 是 |
> | diskencryptionsets | 否 | 否 |
> | disks | 是 | 是 |
> | galleries | 否 | 否 |
> | galleries/images | 否 | 否 |
> | galleries/images/versions | 否 | 否 |
> | hostgroups | 否 | 否 |
> | hostgroups/hosts | 否 | 否 |
> | images | 是 | 是 |
> | proximityplacementgroups | 否 | 否 |
> | restorepointcollections | 否 | 否 |
> | sharedvmimages | 否 | 否 |
> | sharedvmimages/versions | 否 | 否 |
> | snapshots | 是 | 是 |
> | virtualmachines | 是 | 是 |
> | virtualmachines/extensions | 是 | 是 |
> | virtualmachinescalesets | 是 | 是 |

> [!IMPORTANT]
> 请参阅[虚拟机移动指南](./move-limitations/virtual-machines-move-limitations.md)。

<!-- Not Available on ## Microsoft.Container-->

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | containergroups | 否 | 否 |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | registries | 是 | 是 |
> | registries/buildtasks | 是 | 是 |
> | registries/replications | 是 | 是 |
> | registries/tasks | 是 | 是 |
> | registries/webhooks | 是 | 是 |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | containerservices | 否 | 否 |
> | managedclusters | 否 | 否 |
> | openshiftmanagedclusters | 否 | 否 |

<!-- Not Available on ## Microsoft.ContentModerator-->
<!-- Not Available on ## Microsoft.CortanaAnalytics-->
<!-- Not Available on ## Microsoft.CostManagement-->
<!-- Not Available on ## Microsoft.CustomerInsights-->
<!-- Not Available on ## Microsoft.DataBox-->
<!-- Not Available on ## Microsoft.DataBoxEdge-->
<!-- Not Available on ## Microsoft.Databricks-->
<!-- Not Available on ## Microsoft.DataCatalog-->
<!-- Not Available on## Microsoft.DataConnect-->
<!-- Not Available on## Microsoft.DataExchange-->

<a name="microsoftdatafactory"></a>
## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | datafactories | 是 | 是 |
> | factories | 是 | 是 |

<!-- Not Available on ## Microsoft.DataLake-->
<!-- Not Available on ## Microsoft.DataLakeAnalytics-->
<!-- Not Available on ## Microsoft.DataLakeStore-->

<a name="microsoftdatamigration"></a>
## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | services | 否 | 否 |
> | services/projects | 否 | 否 |
> | slots | 否 | 否 |

<a name="microsoftdbformariadb"></a>
## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | servers | 是 | 是 |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | servers | 是 | 是 |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | servergroups | 否 | 否 |
> | servers | 是 | 是 |
> | serversv2 | 是 | 是 |

<!--Not Available on ## Microsoft.DeploymentManager-->

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | elasticpools | 否 | 否 |
> | elasticpools/iothubtenants | 否 | 否 |
> | iothubs | 是 | 是 |
> | provisioningservices | 是 | 是 |

<!-- Not Available on ## Microsoft.DevSpaces-->
<!-- Not Available on ## Microsoft.DevTestLab-->

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | databaseaccounts | 是 | 是 |

<!-- Not Available on ## Microsoft.DomainRegistration-->
<!-- Not Available on ## Microsoft.EnterpriseKnowledgeGraph-->

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | domains | 是 | 是 |
> | eventSubscriptions | 否 - 无法独立移动，但会自动随已订阅的资源移动。 | 否 - 无法独立移动，但会自动随已订阅的资源移动。 |
> | topics | 是 | 是 |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | clusters | 是 | 是 |
> | namespaces | 是 | 是 |

<!-- Not Available on ## Microsoft.Genomics-->
<!-- Not Available on ## Microsoft.HanaOnAzure-->

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | clusters | 是 | 是 |

> [!IMPORTANT]
> 可以将 HDInsight 群集移到新订阅或资源组。 但是，无法在订阅之间移动链接到 HDInsight 群集的网络资源（例如虚拟网络、NIC 或负载均衡器）。 此外，无法将连接到群集的虚拟机的 NIC 移到新的资源组。
>
> 将 HDInsight 群集移至新订阅时，请先移动其他资源（例如存储帐户）。 然后移动 HDInsight 群集本身。

<!-- Not Available on ## Microsoft.HealthcareApis-->
<!-- Not Available on ## Microsoft.HybridCompute-->
<!-- Not Available on ## Microsoft.HybridData-->

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | jobs | 是 | 是 |

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |
> | actiongroups | 是 | 是 |
> | activitylogalerts | 否 | 否 |
> | alertrules | 是 | 是 |
> | autoscalesettings | 是 | 是 |
> | components | 是 | 是 |
> | guestdiagnosticsettings | 否 | 否 |
> | metricalerts | 否 | 否 |
> | notificationgroups | 否 | 否 |
> | notificationrules | 否 | 否 |
> | scheduledqueryrules | 是 | 是 |
> | webtests | 是 | 是 |
> | workbooks | 是 | 是 |

> [!IMPORTANT]
> 确保移到新订阅时，不会超出[订阅配额](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-monitor-limits)。

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | iotapps | 是 | 是 |

<!-- Not Available on ## Microsoft.IoTSpaces-->

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | vaults | 是 | 是 |

<!--MOONCAKE: HSM IS NOT SUPPORT ON MOONCAKE-->

> [!IMPORTANT]
> 用于磁盘加密的 Key Vault 不能移到同一订阅中的资源组，也不能跨订阅移动。

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | clusters | 是 | 是 |

<!-- Not Available on ## Microsoft.LabServices-->
<!-- Not Available on ## Microsoft.LocationBasedServices-->
<!-- Not Available on ## Microsoft.LocationServices-->

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | 否 | 否 |
> | integrationaccounts | 是 | 是 |
> | integrationserviceenvironments | 否 | 否 |
> | isolatedenvironments | 否 | 否 |
> | workflows | 是 | 是 |

<!-- Not Available on ## Microsoft.MachineLearning-->
<!-- Not Available on ## Microsoft.MachineLearningCompute-->
<!-- Not Available on ## Microsoft.MachineLearningExperimentation-->
<!-- Not Available on ## Microsoft.MachineLearningModelManagement-->
<!-- Not Available on ## Microsoft.MachineLearningOperationalization-->
<!-- Not Available on ## Microsoft.MachineLearningServices-->

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | userassignedidentities | 否 | 否 |

<!-- Not Available on ## Microsoft.Maps-->
<!-- Not Available on ## Microsoft.MarketplaceApps-->

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | mediaservices | 是 | 是 |
> | mediaservices/liveevents | 是 | 是 |
> | mediaservices/streamingendpoints | 是 | 是 |

<!-- Not Available on ## Microsoft.Migrate-->
<!-- Not Available on ## Microsoft.NetApp-->

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | applicationgateways | 否 | 否 |
> | applicationgatewaywebapplicationfirewallpolicies | 否 | 否 |
> | applicationsecuritygroups | 是 | 是 |
> | azurefirewalls | 是 | 是 |
> | bastionhosts | 否 | 否 |
> | connections | 是 | 是 |
> | ddoscustompolicies | 是 | 是 |
> | ddosprotectionplans | 否 | 否 |
> | dnszones | 是 | 是 |
> | expressroutecircuits | 否 | 否 |
> | expressroutecrossconnections | 否 | 否 |
> | expressroutegateways | 否 | 否 |
> | expressrouteports | 否 | 否 |
> | frontdoors | 否 | 否 |
> | frontdoorwebapplicationfirewallpolicies | 否 | 否 |
> | loadbalancers | 是 - 基本 SKU<br />否 - 标准 SKU | 是 - 基本 SKU<br />否 - 标准 SKU |
> | localnetworkgateways | 是 | 是 |
> | natgateways | 是 | 是 |
> | networkintentpolicies | 是 | 是 |
> | networkinterfaces | 是 | 是 |
> | networkprofiles | 否 | 否 |
> | networksecuritygroups | 是 | 是 |
> | networkwatchers | 是 | 是 |
> | networkwatchers/connectionmonitors | 是 | 是 |
> | networkwatchers/lenses | 是 | 是 |
> | networkwatchers/pingmeshes | 是 | 是 |
> | p2svpngateways | 否 | 否 |
> | privatednszones | 是 | 是 |
> | privatednszones/virtualnetworklinks | 是 | 是 |
> | privateendpoints | 否 | 否 |
> | privatelinkservices | 否 | 否 |
> | publicipaddresses | 是 - 基本 SKU<br />否 - 标准 SKU | 是 - 基本 SKU<br />否 - 标准 SKU |
> | publicipprefixes | 是 | 是 |
> | routefilters | 否 | 否 |
> | routetables | 是 | 是 |
> | securegateways | 是 | 是 |
> | serviceendpointpolicies | 是 | 是 |
> | trafficmanagerprofiles | 是 | 是 |
> | virtualhubs | 否 | 否 |
> | virtualnetworkgateways | 是 | 是 |
> | virtualnetworks | 是 | 是 |
> | virtualnetworktaps | 否 | 否 |
> | virtualwans | 否 | 否 |
> | vpngateways（虚拟 WAN） | 否 | 否 |
> | vpnsites（虚拟 WAN） | 否 | 否 |
> | webapplicationfirewallpolicies | 是 | 是 |

> [!IMPORTANT]
> 请参阅[网络移动指南](./move-limitations/networking-move-limitations.md)。

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | namespaces | 是 | 是 |
> | namespaces/notificationhubs | 是 | 是 |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | workspaces | 是 | 是 |

> [!IMPORTANT]
> 确保移到新订阅时，不会超出[订阅配额](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-monitor-limits)。

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | managementconfigurations | 是 | 是 |
> | solutions | 是 | 是 |
> | 视图 | 是 | 是 |

<!--Not Available on ## Microsoft.Peering-->

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | dashboards | 是 | 是 |

<a name="microsoftportalsdk"><a/>
<!--Not Available on ## Microsoft.PortalSdk-->

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | workspacecollections | 是 | 是 |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | capacities | 是 | 是 |

<!--Not Available on ## Microsoft.ProjectOxford-->

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | vaults | 是 | 是 |

> [!IMPORTANT]
> 请参阅[恢复服务移动指南](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure-resource-manager/toc.json)。

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | namespaces | 是 | 是 |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 查询 | 是 | 是 |

<!--Not Available on ## Microsoft.SaaS-->

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | flows | 是 | 是 |
> | jobcollections | 是 | 是 |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | searchservices | 是 | 是 |

> [!IMPORTANT]
> 不能通过一项操作移动不同区域中的多个搜索资源。 只能通过多个单独的操作移动它们。

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | iotsecuritysolutions | 是 | 是 |
> | playbookconfigurations | 否 | 否 |

<!-- Not Available on ## Microsoft.ServerManagement-->

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | namespaces | 是 | 是 |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | applications | 否 | 否 |
> | clusters | 是 | 是 |
> | clusters/applications | 否 | 否 |
> | containergroups | 否 | 否 |
> | containergroupsets | 否 | 否 |
> | edgeclusters | 否 | 否 |
> | networks | 否 | 否 |
> | secretstores | 否 | 否 |
> | volumes | 否 | 否 |

<!-- Not Available on ## Microsoft.ServiceFabricMesh-->
<!-- Not Available on ## Microsoft.SignalRService-->

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | signalr | 是 | 是 |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | appliancedefinitions | 否 | 否 |
> | appliances | 否 | 否 |
> | applicationdefinitions | 否 | 否 |
> | applications | 否 | 否 |
> | jitrequests | 否 | 否 |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | instancepools | 否 | 否 |
> | managedinstances | 否 | 否 |
> | managedinstances/databases | 否 | 否 |
> | servers | 是 | 是 |
> | servers/databases | 是 | 是 |
> | servers/elasticpools | 是 | 是 |
> | virtualclusters | 是 | 是 |

> [!IMPORTANT]
> 数据库和服务器必须位于同一个资源组中。 移动 SQL 服务器时，也会移动其所有数据库。 此行为适用于 Azure SQL 数据库和 Azure SQL 数据仓库数据库。

<!-- Not Available on ## Microsoft.SqlVirtualMachine-->
<!-- Not Available on ## Microsoft.SqlVM-->

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | storageaccounts | 是 | 是 |

<!-- Not Available on ## Microsoft.StorageCache-->
<!-- Not Available on ## Microsoft.StorageSync-->
<!-- Not Available on ## Microsoft.StorageSyncDev-->
<!-- Not Available on ## Microsoft.StorageSyncInt-->
<!-- Not Available on ## Microsoft.StorSimple-->

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | streamingjobs | 是 | 是 |

> [!IMPORTANT]
> 当流分析作业处于运行状态时，则无法进行移动。

<!-- Not Available on ## Microsoft.StreamAnalyticsExplorer-->
<!-- Not Available on ## Microsoft.TerraformOSS-->

<a name="microsofttimeseriesinsights"></a>
## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | environments | 是 | 是 |
> | environments/eventsources | 是 | 是 |
> | environments/referencedatasets | 是 | 是 |

<!-- Not Available on ## Microsoft.Token-->
<!-- Not Available on ## Microsoft.VirtualMachineImages-->
<!-- Not Available on ## microsoft.visualstudio-->
<!-- Not Available on ## Microsoft.VMwareCloudSimple-->

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | certificates | 否 | 是 |
> | connectiongateways | 是 | 是 |
> | connections | 是 | 是 |
> | customapis | 是 | 是 |
> | hostingenvironments | 否 | 否 |
> | serverfarms | 是 | 是 |
> | sites | 是 | 是 |
> | sites/premieraddons | 是 | 是 |
> | sites/slots | 是 | 是 |

> [!IMPORTANT]
> 请参阅[应用服务移动指南](./move-limitations/app-service-move-limitations.md)。

<!-- Not Available on ## Microsoft.WindowsIoT-->
<!-- Not Available on ## Microsoft.WindowsVirtualDesktop-->

## <a name="third-party-services"></a>第三方服务

第三方服务目前不支持移动操作。

## <a name="next-steps"></a>后续步骤
有关用于移动资源的命令，请参阅[将资源移到新资源组或订阅](move-resource-group-and-subscription.md)。

<!--Not Avaialble on [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)-->
<!-- Update_Description: update meta properties, wording update, update link -->