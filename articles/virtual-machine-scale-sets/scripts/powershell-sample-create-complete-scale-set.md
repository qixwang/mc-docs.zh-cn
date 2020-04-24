---
title: Azure PowerShell 示例 - 创建完整的虚拟机规模集 | Microsoft Docs
description: Azure PowerShell 示例
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 05/29/2018
ms.date: 05/16/2019
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: 856a2bc7dc054c2a91a9fc03785f76b62788edc5
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "65917449"
---
# <a name="create-a-complete-virtual-machine-scale-set-with-powershell"></a>使用 PowerShell 创建完整的虚拟机规模集

此脚本创建运行 Windows Server 2016 的虚拟机规模集。 单个资源是配置和创建的，而不是使用 [New-AzVmss 中提供的内置资源创建选项](powershell-sample-create-simple-scale-set.md)。 运行脚本后，可通过 RDP 访问 VM 实例。


如果没有 Azure 订阅，可在开始前创建一个 [试用帐户](https://www.azure.cn/pricing/1rmb-trial) 。

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>示例脚本
```powershell
# Provide your own secure password for use with the VM instances
$cred = Get-Credential

# Define variables for resource names
$resourceGroupName = "myResourceGroup"
$scaleSetName = "myScaleSet"
$location = "ChinaNorth"

# Create a resource group
New-AzureRmResourceGroup -ResourceGroupName $resourceGroupName -Location $location

# Create a virtual network subnet
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $resourceGroupName `
  -Name "myVnet" `
  -Location $location `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet

# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName $resourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -Name "myPublicIP"

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name "myFrontEndPool" `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"

# Create a Network Address Translation (NAT) pool
# A rule for Remote Desktop Protocol (RDP) traffic is created on TCP port 3389
$inboundNATPool = New-AzureRmLoadBalancerInboundNatPoolConfig `
  -Name "myRDPRule" `
  -FrontendIpConfigurationId $frontendIP.Id `
  -Protocol TCP `
  -FrontendPortRangeStart 50001 `
  -FrontendPortRangeEnd 50010 `
  -BackendPort 3389

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName $resourceGroupName `
  -Name "myLoadBalancer" `
  -Location $location `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool `
  -InboundNatPool $inboundNATPool

# Create a load balancer health probe for TCP port 80
Add-AzureRmLoadBalancerProbeConfig -Name "myHealthProbe" `
  -LoadBalancer $lb `
  -Protocol TCP `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port TCP 80
# The health probe from the previous step is used to make sure that traffic is
# only directed to healthy VM instances
Add-AzureRmLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol TCP `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe (Get-AzureRmLoadBalancerProbeConfig -Name "myHealthProbe" -LoadBalancer $lb)

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb

# Create IP address configurations
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -LoadBalancerInboundNatPoolsId $inboundNATPool.Id `
  -SubnetId $vnet.Subnets[0].Id

# Create a config object
# The VMSS config object stores the core information for creating a scale set
$vmssConfig = New-AzureRmVmssConfig `
    -Location $location `
    -SkuCapacity 2 `
    -SkuName "Standard_A1_V2" `
    -UpgradePolicyMode "Automatic"

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -OsDiskCreateOption "FromImage" `
  -ImageReferencePublisher "MicrosoftWindowsServer" `
  -ImageReferenceOffer "WindowsServer" `
  -ImageReferenceSku "2016-Datacenter" `
  -ImageReferenceVersion "latest"

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword "Azure123456!" `
  -ComputerNamePrefix "myVM"

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName $resourceGroupName `
  -Name $scaleSetName `
  -VirtualMachineScaleSet $vmssConfig
```

## <a name="clean-up-deployment"></a>清理部署
运行以下命令可删除资源组、规模集和所有相关资源。

```powershell
Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明
此脚本使用以下命令创建部署。 表中的每一项均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | 创建子网配置。 在虚拟网络创建过程中将使用此配置。 |
| [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) | 创建虚拟网络。 |
| [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) | 创建公共 IP 地址。 |
| [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) | 创建负载均衡器的前端 IP 配置。 |
| [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) | 创建负载均衡器的后端地址池配置。 |
| [New-AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) | 创建负载均衡器的入站 NAT 规则配置。 |
| [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer) | 创建负载均衡器。 |
| [Add-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig) | 创建负载均衡器的探测配置。 |
| [Add-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig) | 创建负载均衡器的规则配置。 |
| [Set-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.Network/Set-azLoadBalancer) | 使用所提供的信息更新负载均衡器。 |
| [New-AzVmssIpConfig](https://docs.microsoft.com/powershell/module/az.Compute/New-azVmssIpConfig) | 为规模集 VM 实例创建 IP 配置。 VM 实例将连接到负载均衡器后端池、NAT 池和虚拟网络子网。 |
| [New-AzVmssConfig](https://docs.microsoft.com/powershell/module/az.Compute/New-azVmssConfig) | 创建规模集配置。 此配置包括要创建的 VM 实例数量、VM SKU（大小）和升级策略模式等信息。 此配置由其他 cmdlet 添加，并在创建规模集时使用。 |
| [Set-AzVmssStorageProfile](https://docs.microsoft.com/powershell/module/az.Compute/Set-azVmssStorageProfile) | 定义要用于 VM 实例的映像，并将其添加到规模集配置中。 |
| [Set-AzVmssOsProfile](https://docs.microsoft.com/powershell/module/az.Compute/Set-azVmssStorageProfile) | 定义管理用户名和密码凭据以及 VM 命名前缀。 将这些值添加到规模集配置。 |
| [Add-AzVmssNetworkInterfaceConfiguration](https://docs.microsoft.com/powershell/module/az.Compute/Add-azVmssNetworkInterfaceConfiguration) | 根据 IP 配置，将虚拟网络接口添加到 VM 实例。 将这些值添加到规模集配置。 |
| [New-AzVmss](https://docs.microsoft.com/powershell/module/az.Compute/New-azVmss) | 根据规模集配置中提供的信息创建规模集。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤
有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure 虚拟机规模集文档](../powershell-samples.md)中找到其他虚拟机规模集 PowerShell 脚本示例。

<!-- Update_Description: wording update -->