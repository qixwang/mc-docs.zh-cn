---
title: 资源类型支持的移动操作
description: 列出可移到新资源组或订阅的 Azure 资源类型。
ms.topic: conceptual
origin.date: 04/06/2020
ms.date: 04/30/2020
ms.author: v-yeche
ms.openlocfilehash: 58194291022d785ae5df69db86a94c8ffcb252d1
ms.sourcegitcommit: 2d8950c6c255361eb6c66406988e25c69cf4e0f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83392171"
---
# <a name="move-operation-support-for-resources"></a>支持移动操作的资源
本文列出某个 Azure 资源类型是否支持移动操作。 它还提供了有关移动资源时要考虑的特殊条件的信息。

跳转到资源提供程序命名空间：
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Blueprint](#microsoftblueprint)
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
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
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

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | tenants | 否 | 否 |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 配置 | 否 | 否 |
> | 建议 | 否 | 否 |
> | 禁止显示 | 否 | 否 |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | actionrules | 是 | 是 |
> | alerts | 否 | 否 |
> | alertssummary | 否 | 否 |
> | smartdetectoralertrules | 是 | 是 |

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

> [!IMPORTANT]
> 无法移动设置为消耗 SKU 的 API 管理服务。

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
> | checkaccess | 否 | 否 |
> | denyassignments | 否 | 否 |
> | findorphanroleassignments | 否 | 否 |
> | 锁定 | 否 | 否 |
> | 权限 | 否 | 否 |
> | policyassignments | 否 | 否 |
> | policydefinitions | 否 | 否 |
> | policysetdefinitions | 否 | 否 |
> | roleassignments | 否 | 否 |
> | roleassignmentsusagemetrics | 否 | 否 |
> | roledefinitions | 否 | 否 |

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
<!-- Not Available on ## Microsoft.Billing-->
<!-- Not Available on ## Microsoft.BingMaps-->
<!-- Not Available on ## Microsoft.BizTalkServices-->
<!-- Not Available on ## Microsoft.Blockchain-->
<!-- Not Available on ## Microsoft.Blockchain-->

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | blueprintassignments | 否 | 否 |
> | blueprints | 否 | 否 |

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
> | cdnwebapplicationfirewallpolicies | 是 | 否 |
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

<!-- Not Available on ## Microsoft.Cognition-->

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
> | proximityplacementgroups | 是 | 是 |
> | restorepointcollections | 否 | 否 |
> | sharedvmextensions | 否 | 否 |
> | sharedvmimages | 否 | 否 |
> | sharedvmimages/versions | 否 | 否 |
> | snapshots | 是 | 是 |
> | sshpublickeys | 否 | 否 |
> | virtualmachines | 是 | 是 |
> | virtualmachines/extensions | 是 | 是 |
> | virtualmachinescalesets | 是 | 是 |

> [!IMPORTANT]
> 请参阅[虚拟机移动指南](./move-limitations/virtual-machines-move-limitations.md)。

<!-- Not Available on ## Microsoft.Consumption-->
<!-- Not Available on ## Microsoft.Container-->

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | containergroups | 否 | 否 |
> | serviceassociationlinks | 否 | 否 |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | registries | 是 | 是 |
> | registries/agentpools | 否 | 否 |
> | registries/buildtasks | 是 | 是 |
> | registries/replications | 是 | 是 |
> | registries/taskruns | 是 | 是 |
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
<!-- Not Available on ## Microsoft.CustomProviders-->
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


<!-- Not Available on ## Microsoft.DataProtection-->
<!-- Not Available on ## Microsoft.DataShare-->

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
> | singleservers | 是 | 是 |

<!--Not Available on ## Microsoft.DeploymentManager-->
<!--Not Available on ## Microsoft.DesktopVirtualization-->

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | elasticpools | 否 | 否 |
> | elasticpools/iothubtenants | 否 | 否 |
> | iothubs | 是 | 是 |
> | provisioningservices | 是 | 是 |

<!--Not Available on ## Microsoft.DevOps-->
<!-- Not Available on ## Microsoft.DevSpaces-->
<!-- Not Available on ## Microsoft.DevTestLab-->

<!--Not Available on ## Microsoft.DigitalTwins-->
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
> | eventsubscriptions | 否 - 无法独立移动，但会自动随已订阅的资源移动。 | 否 - 无法独立移动，但会自动随已订阅的资源移动。 |
> | extensiontopics | 否 | 否 |
> | partnernamespaces | 是 | 是 |
> | partnerregistrations | 否 | 否 |
> | partnertopics | 是 | 是 |
> | systemtopics | 是 | 是 |
> | topics | 是 | 是 |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | clusters | 是 | 是 |
> | namespaces | 是 | 是 |

<!-- Not Available on ## Microsoft.Experimentation-->
<!-- Not Available on ## Microsoft.Falcon-->
<!-- Not Available on ## Microsoft.Genomics-->

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | automanagedaccounts | 否 | 否 |
> | automanagedvmconfigurationprofiles | 否 | 否 |
> | guestconfigurationassignments | 否 | 否 |
> | software | 否 | 否 |
> | softwareupdateprofile | 否 | 否 |
> | softwareupdates | 否 | 否 |

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
<!-- Not Available on ## Microsoft.Hydra-->

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | jobs | 是 | 是 |

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | actiongroups | 是 | 是 |
> | activitylogalerts | 否 | 否 |
> | alertrules | 是 | 是 |
> | autoscalesettings | 是 | 是 |
> | baseline | 否 | 否 |
> | calculatebaseline | 否 | 否 |
> | components | 是 | 是 |
> | datacollectionrules | 否 | 否 |
> | diagnosticsettings | 否 | 否 |
> | diagnosticsettingscategories | 否 | 否 |
> | eventtypes | 否 | 否 |
> | extendeddiagnosticsettings | 否 | 否 |
> | guestdiagnosticsettings | 否 | 否 |
> | logdefinitions | 否 | 否 |
> | 日志 | 否 | 否 |
> | metricalerts | 否 | 否 |
> | metricbaselines | 否 | 否 |
> | metricdefinitions | 否 | 否 |
> | metricnamespaces | 否 | 否 |
> | 指标 | 否 | 否 |
> | myworkbooks | 否 | 否 |
> | notificationgroups | 否 | 否 |
> | privatelinkscopes | 是 | 是 |
> | scheduledqueryrules | 是 | 是 |
> | 拓扑 | 否 | 否 |
> | 事务 | 否 | 否 |
> | vminsightsonboardingstatuses | 否 | 否 |
> | webtests | 是 | 是 |
> | workbooks | 是 | 是 |
> | workbooktemplates | 是 | 是 |

> [!IMPORTANT]
> 确保移到新订阅时，不会超出[订阅配额](azure-subscription-service-limits.md#azure-monitor-limits)。

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

<!-- Not Available on ## Microsoft.Kubernetes-->

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
> | integrationserviceenvironments | 是 | 否 |
> | integrationserviceenvironments/managedapis | 是 | 否 |
> | isolatedenvironments | 否 | 否 |
> | workflows | 是 | 是 |

<!-- Not Available on ## Microsoft.MachineLearning-->
<!-- Not Available on ## Microsoft.MachineLearningCompute-->
<!-- Not Available on ## Microsoft.MachineLearningExperimentation-->
<!-- Not Available on ## Microsoft.MachineLearningModelManagement-->
<!-- Not Available on ## Microsoft.MachineLearningOperationalization-->

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | workspaces | 否 | 否 |
> | workspaces / computes | 否 | 否 |

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | applyupdates | 否 | 否 |
> | configurationassignments | 否 | 否 |
> | maintenanceconfigurations | 是 | 是 |
> | updates | 否 | 否 |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | identities | 否 | 否 |
> | userassignedidentities | 否 | 否 |


<!-- Not Available on ## Microsoft.ManagedNetwork-->
<!-- Not Available on ## Microsoft.ManagedServices-->
<!-- Not Available on ## Microsoft.Maps-->
<!-- Not Available on ## Microsoft.MarketplaceApps-->

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | mediaservices | 是 | 是 |
> | mediaservices/liveevents | 是 | 是 |
> | mediaservices/streamingendpoints | 是 | 是 |

<!-- Not Available on ## Microsoft.Microservices4Spring-->
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
> | expressroutegateways | 否 | 否 |
> | firewallpolicies | 是 | 是 |
> | frontdoors | 否 | 否 |
> | frontdoorwebapplicationfirewallpolicies | 否 | 否 |
> | ipgroups | 是 | 是 |
> | loadbalancers | 是 - 基本 SKU<br />否 - 标准 SKU | 是 - 基本 SKU<br />否 - 标准 SKU |
> | localnetworkgateways | 是 | 是 |
> | networkexperimentprofiles | 是 | 是 |
> | networkintentpolicies | 是 | 是 |
> | networkinterfaces | 是 | 是 |
> | networkprofiles | 否 | 否 |
> | networksecuritygroups | 是 | 是 |
> | networkwatchers | 是 | 否 |
> | networkwatchers/connectionmonitors | 是 | 否 |
> | networkwatchers/flowlogs | 是 | 否 |
> | networkwatchers/pingmeshes | 是 | 否 |
> | p2svpngateways | 否 | 否 |
> | privatednszones | 是 | 是 |
> | privatednszones/virtualnetworklinks | 是 | 是 |
> | privateendpointredirectmaps | 否 | 否 |
> | privateendpoints | 是 | 是 |
> | privatelinkservices | 否 | 否 |
> | publicipaddresses | 是 - 基本 SKU<br />否 - 标准 SKU | 是 - 基本 SKU<br />否 - 标准 SKU |
> | publicipprefixes | 是 | 是 |
> | routefilters | 否 | 否 |
> | routetables | 是 | 是 |
> | serviceendpointpolicies | 是 | 是 |
> | trafficmanagerprofiles | 是 | 是 |
> | virtualhubs | 否 | 否 |
> | virtualnetworkgateways | 是 | 是 |
> | virtualnetworks | 是 | 是 |
> | virtualnetworktaps | 否 | 否 |
> | virtualrouters | 是 | 是 |
> | virtualwans | 否 | 否 |
> | vpngateways（虚拟 WAN） | 否 | 否 |
> | vpnserverconfigurations | 否 | 否 |
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

<!-- Not Available on ## Microsoft.ObjectStore-->
## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | clusters | 否 | 否 |
> | storageinsightconfigs | 否 | 否 |
> | workspaces | 是 | 是 |

> [!IMPORTANT]
> 确保移到新订阅时，不会超出[订阅配额](azure-subscription-service-limits.md#azure-monitor-limits)。

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | managementassociations | 否 | 否 |
> | managementconfigurations | 是 | 是 |
> | solutions | 是 | 是 |
> | 视图 | 是 | 是 |

<!--Not Available on ## Microsoft.Peering-->

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | policyevents | 否 | 否 |
> | policystates | 否 | 否 |
> | policytrackedresources | 否 | 否 |
> | remediations | 否 | 否 |

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

<!--Not Available on ## Microsoft.ProjectBabylon-->
<!--Not Available on ## Microsoft.ProjectOxford-->
<!--Not Available on ## Microsoft.ProviderHub-->
<!--Not Available on ## Microsoft.Quantum-->

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | backupprotecteditems | 否 | 否 |
> | replicationeligibilityresults | 否 | 否 |
> | vaults | 是 | 是 |

<!--Not Available on [Recovery Services move guidance](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure-resource-manager/toc.json)-->

<!--Not Available on ## Microsoft.RedHatOpenShift-->


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

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | availabilitystatuses | 否 | 否 |
> | childavailabilitystatuses | 否 | 否 |
> | childresources | 否 | 否 |
> | events | 否 | 否 |
> | 通知 | 否 | 否 |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | deploymentscripts | 否 | 否 |
> | 链接 | 否 | 否 |
> | tags | 否 | 否 |

<!--Not Available on ## Microsoft.SaaS-->

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
> | adaptivenetworkhardenings | 否 | 否 |
> | advancedthreatprotectionsettings | 否 | 否 |
> | assessmentmetadata | 否 | 否 |
> | assessments | 否 | 否 |
> | automations | 是 | 是 |
> | complianceresults | 否 | 否 |
> | compliances | 否 | 否 |
> | datacollectionagents | 否 | 否 |
> | devicesecuritygroups | 否 | 否 |
> | informationprotectionpolicies | 否 | 否 |
> | iotsecuritysolutions | 是 | 是 |
> | servervulnerabilityassessments | 否 | 否 |

<!-- Not Available on ## Microsoft.SecurityInsights-->

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
> | managedclusters | 否 | 否 |
> | networks | 否 | 否 |
> | secretstores | 否 | 否 |
> | volumes | 否 | 否 |

<!-- Not Available on ## Microsoft.ServiceFabricMesh-->
<!-- Not Available on ## Microsoft.SignalRService-->
<!-- Not Available on ## Microsoft.Services-->

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | signalr | 是 | 是 |

<!-- Not Available on ## Microsoft.SoftwarePlan-->

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
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
> | servers/jobaccounts | 是 | 是 |
> | servers/jobagents | 是 | 是 |
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
<!-- Not Available on ## Microsoft.Subscription-->
<!-- Not Available on ## microsoft.support-->
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
<!-- Not Available on ## Microsoft.VnfManager-->
<!-- Not Available on ## Microsoft.VSOnline-->

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | certificates | 否 | 是 |
> | connectiongateways | 是 | 是 |
> | connections | 是 | 是 |
> | customapis | 是 | 是 |
> | hostingenvironments | 否 | 否 |
> | kubeenvironments | 是 | 是 |
> | serverfarms | 是 | 是 |
> | sites | 是 | 是 |
> | sites/premieraddons | 是 | 是 |
> | sites/slots | 是 | 是 |
> | staticsites | 否 | 否 |

> [!IMPORTANT]
> 请参阅[应用服务移动指南](./move-limitations/app-service-move-limitations.md)。

<!-- Not Available on ## Microsoft.WindowsESU-->
<!-- Not Available on ## Microsoft.WindowsIoT-->
<!-- Not Available on ## Microsoft.WorkloadMonitor-->

## <a name="third-party-services"></a>第三方服务

第三方服务目前不支持移动操作。

## <a name="next-steps"></a>后续步骤
有关用于移动资源的命令，请参阅[将资源移到新资源组或订阅](move-resource-group-and-subscription.md)。

<!--Not Avaialble on [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)-->
<!-- Update_Description: update meta properties, wording update, update link -->