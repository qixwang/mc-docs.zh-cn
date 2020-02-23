---
title: 在 PowerShell 的负载均衡器中打开应用程序端口
description: Azure PowerShell 脚本示例 - 打开 Azure 负载均衡器中 Service Fabric 应用程序的端口。
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
origin.date: 05/18/2018
ms.date: 02/24/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 6a21396005d2b4b2c440461d75a7945f3121ebf1
ms.sourcegitcommit: afe972418a883551e36ede8deae32ba6528fb8dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77540513"
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>打开 Azure 负载均衡器中的应用程序端口

正在 Azure 中运行的 Service Fabric 应用程序位于 Azure 负载均衡器后。 此示例脚本将打开 Azure 负载均衡器中的端口，以便 Service Fabric 应用程序可与外部客户端通信。 根据需要自定义参数。 如果群集在某个网络安全组中，则还要[添加入站网络安全组规则](service-fabric-powershell-add-nsg-rule.md)来允许入站流量。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

必要时，使用 [Service Fabric SDK](../service-fabric-get-started.md) 安装 Service Fabric PowerShell 模块。 

## <a name="sample-script"></a>示例脚本

```powershell
# Variables
$probename = "AppPortProbe6"
$rulename="AppPortLBRule6"
$RGname="mysftestclustergroup"
$port=8303
$subscriptionID = 'subscription ID'

# Login and select your subscription
Connect-AzAccount -Environment AzureChinaCloud
Get-AzSubscription -SubscriptionId $subscriptionID | Select-AzSubscription 

# Get the load balancer resource
$resource = Get-AzResource | Where {$_.ResourceGroupName -eq $RGname -and $_.ResourceType -eq "Microsoft.Network/loadBalancers"} 
$slb = Get-AzLoadBalancer -Name $resource.Name -ResourceGroupName $RGname

# Add a new probe configuration to the load balancer
$slb | Add-AzLoadBalancerProbeConfig -Name $probename -Protocol Tcp -Port $port -IntervalInSeconds 15 -ProbeCount 2

# Add rule configuration to the load balancer
$probe = Get-AzLoadBalancerProbeConfig -Name $probename -LoadBalancer $slb
$slb | Add-AzLoadBalancerRuleConfig -Name $rulename -BackendAddressPool $slb.BackendAddressPools[0] -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -Probe $probe -Protocol Tcp -FrontendPort $port -BackendPort $port

# Set the goal state for the load balancer
$slb | Set-AzLoadBalancer

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | 获取 Azure 资源。  |
| [Get-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer) | 获取 Azure 负载均衡器。 |
| [Add-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/add-azloadbalancerprobeconfig) | 向负载均衡器添加探测配置。|
| [Get-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancerprobeconfig) | 获取负载均衡器的探测配置。 |
| [Add-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/add-azloadbalancerruleconfig) | 向负载均衡器添加规则配置。 |
| [Set-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/set-azloadbalancer) | 设置负载均衡器的目标状态。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../service-fabric-powershell-samples.md)中找到 Azure Service Fabric 的其他 Powershell 示例。

<!--Update_Description: update meta properties, update link -->