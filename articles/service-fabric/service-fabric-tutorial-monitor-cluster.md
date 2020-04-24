---
title: 在 Azure 中监视 Service Fabric 群集
description: 在本教程中，你将学习如何通过查看 Service Fabric 事件、查询 EventStore API、监视 perf 计数器以及查看运行状况报告来监视群集。
author: rockboyfor
ms.topic: tutorial
origin.date: 07/22/2019
ms.date: 01/13/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 955f3fa74f7d1e304ba9dea10cb2759235f754f8
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75742008"
---
# <a name="tutorial-monitor-a-service-fabric-cluster-in-azure"></a>教程：在 Azure 中监视 Service Fabric 群集

在任何云环境中开发、测试和部署工作负荷时，监视和诊断至关重要。 本教程是一个系列的第二部分，介绍如何使用事件、性能计数器和运行状况报告来监视和诊断 Service Fabric 群集。   有关更多信息，请阅读有关[群集监视](service-fabric-diagnostics-overview.md#platform-cluster-monitoring)和[基础结构监视](service-fabric-diagnostics-overview.md#infrastructure-performance-monitoring)的概述。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 查看 Service Fabric 事件
> * 查询群集事件的 EventStore API
> * 监视基础架构/收集性能计数器
> * 查看群集运行状况报告

在此系列教程中，你将学习如何：
> [!div class="checklist"]
> * 使用模板在 Azure 上创建安全 [Windows 群集](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
> * 监视群集
> * [缩小或扩大群集](service-fabric-tutorial-scale-cluster.md)
> * [升级群集的运行时](service-fabric-tutorial-upgrade-cluster.md)
> * [删除群集](service-fabric-tutorial-delete-cluster.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>先决条件

在开始学习本教程之前：

* 如果还没有 Azure 订阅，请创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)
* 安装 [Azure Powershell](https://docs.microsoft.com/powershell/azure/install-Az-ps) 或 [Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。
* 创建安全的 [Windows 群集](service-fabric-tutorial-create-vnet-and-windows-cluster.md) 
* 为群集设置[诊断集合](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configurediagnostics_anchor)
* 在群集中启用 [EventStore 服务](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configureeventstore_anchor)
* 配置群集的 [Azure Monitor 日志和 Log Analytics 代理](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configureloganalytics_anchor)

<!--Not Avaialble on Service Fabric Analytics-->
<!--Not Avaialble on ## View Service Fabric events using Azure Monitor logs-->
<!--Not Avaialble on ### View Service Fabric Events, including actions on nodes-->
<!--Not Avaialble on ### View Service Fabric application events-->
<!--Not Available on Service Fabric Analytics-->
<!--Not Avaialble on ## View performance counters with Azure Monitor logs-->

## <a name="query-the-eventstore-service"></a>查询 EventStore 服务
[EventStore 服务](service-fabric-diagnostics-eventstore.md)提供了在给定时间点了解群集或工作负载状态的方法。 EventStore 是有状态 Service Fabric 服务，它维护群集中的事件。 事件通过 [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)、REST 和 API 公开。 EventStore 直接查询群集以获取群集中任何实体的诊断数据，要查看 EventStore 中可用事件的完整列表，请参阅 [Service Fabric 事件](service-fabric-diagnostics-event-generation-operational.md)。

可以使用 [Service Fabric 客户端库](https://docs.azure.cn/dotnet/api/overview/service-fabric?view=azure-dotnet#client-library)以编程方式查询 EventStore API。

以下是通过 GetClusterEventListAsync 函数查询 2018-04-03T18:00:00Z 和 2018-04-04T18:00:00Z 之间的所有群集事件的示例请求。

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

以下是另一个查询 2018 年 9 月群集运行状况和所有节点事件并将其打印出来的示例。

```csharp
const int timeoutSecs = 60;
var clusterUrl = new Uri(@"http://localhost:19080"); // This example is for a Local cluster
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl);

var clusterHealth = sfhttpClient.Cluster.GetClusterHealthAsync().GetAwaiter().GetResult();
Console.WriteLine("Cluster Health: {0}", clusterHealth.AggregatedHealthState.Value.ToString());

Console.WriteLine("Querying for node events...");
var nodesEvents = sfhttpClient.EventsStore.GetNodesEventListAsync(
    "2018-09-01T00:00:00Z",
    "2018-09-30T23:59:59Z",
    timeoutSecs,
    "NodeDown,NodeUp")
    .GetAwaiter()
    .GetResult()
    .ToList();
Console.WriteLine("Result Count: {0}", nodesEvents.Count());

foreach (var nodeEvent in nodesEvents)
{
    Console.Write("Node event happened at {0}, Node name: {1} ", nodeEvent.TimeStamp, nodeEvent.NodeName);
    if (nodeEvent is NodeDownEvent)
    {
        var nodeDownEvent = nodeEvent as NodeDownEvent;
        Console.WriteLine("(Node is down, and it was last up at {0})", nodeDownEvent.LastNodeUpAt);
    }
    else if (nodeEvent is NodeUpEvent)
    {
        var nodeUpEvent = nodeEvent as NodeUpEvent;
        Console.WriteLine("(Node is up, and it was last down at {0})", nodeUpEvent.LastNodeDownAt);
    }
}
```

## <a name="monitor-cluster-health"></a>监视群集运行状况
Service Fabric 引入了一种具有运行状况实体的[运行状况模型](service-fabric-health-introduction.md)，系统组件和监视器可以在其上报告它们监视的本地状况。 [运行状况存储](service-fabric-health-introduction.md#health-store)聚合所有运行状况数据以确定实体是否正常运行。

群集会自动被系统组件发送的运行状况报告所填充。 从[使用系统运行状况报告进行故障排除](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)了解更多信息。

Service Fabric 为每个支持的[实体类型](service-fabric-health-introduction.md#health-entities-and-hierarchy)提供运行状况查询。 可以通过 API（使用 [FabricClient.HealthManager](https://docs.azure.cn/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet) 上的方法）、PowerShell cmdlet 和 REST 访问它们。 这些查询返回有关实体的完整运行状况信息：聚合运行状况、实体运行状况事件、子运行状况（在适用时）、不正常评估（实体不正常时）以及子集运行状况统计信息（在适用时）。

### <a name="get-cluster-health"></a>获取群集运行状况
[Get-ServiceFabricClusterHealth cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth) 返回群集实体的运行状况，并包含应用程序和节点（群集的子项）的运行状况。  首先使用 [Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet 连接到群集。

群集的状态是 11 个节点、系统应用程序和 fabric:/Voting 按所述进行配置。

以下示例使用默认运行状况策略获取群集运行状况。 11 个节点处于正常状态，但是群集的聚合运行状况是错误，因为 fabric:/Voting 应用程序处于错误状态。 请注意不正常评估如何提供触发聚合运行状况的详细条件。

```powershell
Get-ServiceFabricClusterHealth

AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          100% (1/1) applications are unhealthy. The evaluation tolerates 0% unhealthy applications.

                          Application 'fabric:/Voting' is in Error.

                            33% (1/3) deployed applications are unhealthy. The evaluation tolerates 0% unhealthy deployed applications.

                            Deployed application on node '_nt2vm_3' is in Error.

                                50% (1/2) deployed service packages are unhealthy.

                                Service package for manifest 'VotingWebPkg' and service package activation ID '8723eb73-9b83-406b-9de3-172142ba15f3' is in Error.

                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376195593305'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1

NodeHealthStates        : 
                          NodeName              : _nt2vm_3
                          AggregatedHealthState : Ok

                          NodeName              : _nt1vm_4
                          AggregatedHealthState : Ok

                          NodeName              : _nt2vm_2
                          AggregatedHealthState : Ok

                          NodeName              : _nt1vm_3
                          AggregatedHealthState : Ok

                          NodeName              : _nt2vm_1
                          AggregatedHealthState : Ok

                          NodeName              : _nt1vm_2
                          AggregatedHealthState : Ok

                          NodeName              : _nt2vm_0
                          AggregatedHealthState : Ok

                          NodeName              : _nt1vm_1
                          AggregatedHealthState : Ok

                          NodeName              : _nt1vm_0
                          AggregatedHealthState : Ok

                          NodeName              : _nt3vm_0
                          AggregatedHealthState : Ok

                          NodeName              : _nt2vm_4
                          AggregatedHealthState : Ok

ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/Voting
                          AggregatedHealthState : Error

HealthEvents            : None
HealthStatistics        : 
                          Node                  : 11 Ok, 0 Warning, 0 Error
                          Replica               : 4 Ok, 0 Warning, 0 Error
                          Partition             : 2 Ok, 0 Warning, 0 Error
                          Service               : 2 Ok, 0 Warning, 0 Error
                          DeployedServicePackage : 3 Ok, 1 Warning, 1 Error
                          DeployedApplication   : 1 Ok, 1 Warning, 1 Error
                          Application           : 0 Ok, 0 Warning, 1 Error
```

以下示例使用自定义应用程序策略获取群集的运行状况。 它筛选结果以只获取有错误或警告的应用程序和节点。 此示例中不会返回任何节点，因为这些节点都是正常的。 仅 fabric:/Voting 应用程序符合应用程序筛选器。 因为自定义策略指定对于 fabric:/Voting 应用程序将警告视为错误，应用程序被评估为错误，从而群集也被评估为错误。

```powershell
$appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/Voting"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics

AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          100% (1/1) applications are unhealthy. The evaluation tolerates 0% unhealthy applications.

                          Application 'fabric:/Voting' is in Error.

                            100% (5/5) deployed applications are unhealthy. The evaluation tolerates 0% unhealthy deployed applications.

                            Deployed application on node '_nt2vm_3' is in Error.

                                50% (1/2) deployed service packages are unhealthy.

                                Service package for manifest 'VotingWebPkg' and service package activation ID '8723eb73-9b83-406b-9de3-172142ba15f3' is in Error.

                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376195593305'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1

                            Deployed application on node '_nt2vm_2' is in Error.

                                50% (1/2) deployed service packages are unhealthy.

                                Service package for manifest 'VotingWebPkg' and service package activation ID '2466f2f9-d5fd-410c-a6a4-5b1e00630cca' is in Error.

                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376486201388'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1

                            Deployed application on node '_nt2vm_4' is in Error.

                                100% (1/1) deployed service packages are unhealthy.

                                Service package for manifest 'VotingWebPkg' and service package activation ID '5faa5201-eede-400a-865f-07f7f886aa32' is in Error.

                                    'System.Hosting' reported Warning for property 'CodePackageActivation:Code:SetupEntryPoint:131959376207396204'. The evaluation treats 
                          Warning as Error.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1

                            Deployed application on node '_nt2vm_0' is in Error.

                                100% (1/1) deployed service packages are unhealthy.

                                Service package for manifest 'VotingWebPkg' and service package activation ID '204f1783-f774-4f3a-b371-d9983afaf059' is in Error.

                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959375885791093'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1

                            Deployed application on node '_nt3vm_0' is in Error.

                                50% (1/2) deployed service packages are unhealthy.

                                Service package for manifest 'VotingWebPkg' and service package activation ID '2533ae95-2d2a-4f8b-beef-41e13e4c0081' is in Error.

                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376108346272'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1                         

NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/Voting
                          AggregatedHealthState : Error

HealthEvents            : None
```

### <a name="get-node-health"></a>获取节点运行状况
[Get-ServiceFabricNodeHealth cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth) 返回节点实体的运行状况，并包含针对该节点报告的运行状况事件。 首先使用 [Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet 连接到群集。 以下示例使用默认运行状况策略获取特定节点的运行状况：

```powershell
Get-ServiceFabricNodeHealth _nt1vm_3
```

以下示例获取群集中所有节点的运行状况：
```powershell
Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize
```

### <a name="get-system-service-health"></a>获取系统服务运行状况 

获取系统服务的聚合运行状况：

```powershell
Get-ServiceFabricService -ApplicationName fabric:/System | Get-ServiceFabricServiceHealth | select ServiceName, AggregatedHealthState | ft -AutoSize
```

## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 查看 Service Fabric 事件
> * 查询群集事件的 EventStore API
> * 监视基础架构/收集性能计数器
> * 查看群集运行状况报告

接下来，请转到下一个教程了解如何缩放群集。
> [!div class="nextstepaction"]
> [缩放群集](service-fabric-tutorial-scale-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json

<!--Update_Description: wording update -->
