---
title: PowerShell：创建托管实例
titleSuffix: Azure SQL Managed Instance
description: 本文提供用于创建托管实例的 Azure PowerShell 示例脚本。
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 03/25/2019
ms.date: 07/13/2020
ms.openlocfilehash: d9b6502f7a4cb55338225b29182381520f30c0d2
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227302"
---
# <a name="use-powershell-to-create-a-managed-instance"></a>使用 PowerShell 创建托管实例

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqlmi.md)]

此 PowerShell 脚本示例在新虚拟网络的专用子网中创建托管实例。 它还为虚拟网络配置路由表和网络安全组。 成功运行脚本后，即可从虚拟网络或本地环境访问托管实例。 请参阅[将 Azure VM 配置为连接到 Azure SQL 数据库托管实例](../connect-vm-instance-configure.md)和[配置从本地到 Azure SQL 托管实例的点到站点连接](../point-to-site-p2s-configure.md)。

> [!IMPORTANT]
> 有关限制，请参阅[支持的区域](../resource-limits.md#supported-regions)。

本教程需要 Azure PowerShell 1.4.0 或更高版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 需要运行 `Connect-AzAccount -Environment AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
$NSnetworkModels = "Microsoft.Azure.Commands.Network.Models"
$NScollections = "System.Collections.Generic"

Connect-AzAccount -Environment AzureChinaCloud
# The SubscriptionId in which to create these objects
$SubscriptionId = ''
# Set the resource group name and location for your managed instance
$resourceGroupName = "myResourceGroup-$(Get-Random)"
$location = "chinaeast2"
# Set the networking values for your managed instance
$vNetName = "myVnet-$(Get-Random)"
$vNetAddressPrefix = "10.0.0.0/16"
$defaultSubnetName = "myDefaultSubnet-$(Get-Random)"
$defaultSubnetAddressPrefix = "10.0.0.0/24"
$miSubnetName = "myMISubnet-$(Get-Random)"
$miSubnetAddressPrefix = "10.0.0.0/24"
#Set the managed instance name for the new managed instance
$instanceName = "myMIName-$(Get-Random)"
# Set the admin login and password for your managed instance
$miAdminSqlLogin = "SqlAdmin"
$miAdminSqlPassword = "ChangeYourAdminPassword1"
# Set the managed instance service tier, compute level, and license mode
$edition = "General Purpose"
$vCores = 8
$maxStorage = 256
$computeGeneration = "Gen5"
$license = "LicenseIncluded" #"BasePrice" or LicenseIncluded if you have don't have SQL Server licence that can be used for AHB discount

# Set subscription context
Set-AzContext -SubscriptionId $SubscriptionId 

# Create a resource group
$resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location -Tag @{Owner="SQLDB-Samples"}

# Configure virtual network, subnets, network security group, and routing table
$networkSecurityGroupMiManagementService = New-AzNetworkSecurityGroup `
                      -Name 'myNetworkSecurityGroupMiManagementService' `
                      -ResourceGroupName $resourceGroupName `
                      -location $location

$routeTableMiManagementService = New-AzRouteTable `
                      -Name 'myRouteTableMiManagementService' `
                      -ResourceGroupName $resourceGroupName `
                      -location $location

$virtualNetwork = New-AzVirtualNetwork `
                      -ResourceGroupName $resourceGroupName `
                      -Location $location `
                      -Name $vNetName `
                      -AddressPrefix $vNetAddressPrefix

                  Add-AzVirtualNetworkSubnetConfig `
                      -Name $miSubnetName `
                      -VirtualNetwork $virtualNetwork `
                      -AddressPrefix $miSubnetAddressPrefix `
                      -NetworkSecurityGroup $networkSecurityGroupMiManagementService `
                      -RouteTable $routeTableMiManagementService |
                  Set-AzVirtualNetwork

$virtualNetwork = Get-AzVirtualNetwork -Name $vNetName -ResourceGroupName $resourceGroupName

$subnet= $virtualNetwork.Subnets[0]

# Create a delegation
$subnet.Delegations = New-Object "$NScollections.List``1[$NSnetworkModels.PSDelegation]"
$delegationName = "dgManagedInstance" + (Get-Random -Maximum 1000)
$delegation = New-AzDelegation -Name $delegationName -ServiceName "Microsoft.Sql/managedInstances"
$subnet.Delegations.Add($delegation)

Set-AzVirtualNetwork -VirtualNetwork $virtualNetwork

$miSubnetConfigId = $subnet.Id



$allowParameters = @{
    Access = 'Allow'
    Protocol = 'Tcp'
    Direction= 'Inbound'
    SourcePortRange = '*'
    SourceAddressPrefix = 'VirtualNetwork'
    DestinationAddressPrefix = '*'
}
$denyInParameters = @{
    Access = 'Deny'
    Protocol = '*'
    Direction = 'Inbound'
    SourcePortRange = '*'
    SourceAddressPrefix = '*'
    DestinationPortRange = '*'
    DestinationAddressPrefix = '*'
}
$denyOutParameters = @{
    Access = 'Deny'
    Protocol = '*'
    Direction = 'Outbound'
    SourcePortRange = '*'
    SourceAddressPrefix = '*'
    DestinationPortRange = '*'
    DestinationAddressPrefix = '*'
}

Get-AzNetworkSecurityGroup `
        -ResourceGroupName $resourceGroupName `
        -Name "myNetworkSecurityGroupMiManagementService" |
    Add-AzNetworkSecurityRuleConfig `
        @allowParameters `
        -Priority 1000 `
        -Name "allow_tds_inbound" `
        -DestinationPortRange 1433 |
    Add-AzNetworkSecurityRuleConfig `
        @allowParameters `
        -Priority 1100 `
        -Name "allow_redirect_inbound" `
        -DestinationPortRange 11000-11999 |
    Add-AzNetworkSecurityRuleConfig `
        @denyInParameters `
        -Priority 4096 `
        -Name "deny_all_inbound" |
    Add-AzNetworkSecurityRuleConfig `
        @denyOutParameters `
        -Priority 4096 `
        -Name "deny_all_outbound" |
    Set-AzNetworkSecurityGroup


# Create credentials
$secpassword = ConvertTo-SecureString $miAdminSqlPassword -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential ($miAdminSqlLogin, $secpassword)

# Create managed instance
New-AzSqlInstance -Name $instanceName `
                      -ResourceGroupName $resourceGroupName -Location $location -SubnetId $miSubnetConfigId `
                      -AdministratorCredential $credential `
                      -StorageSizeInGB $maxStorage -VCore $vCores -Edition $edition `
                      -ComputeGeneration $computeGeneration -LicenseType $license

# Clean up deployment 
# Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令中的某些命令。 有关下表中使用的命令和其他命令的详细信息，请单击命令特定文档的链接。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。
| [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) | 创建虚拟网络。 |
| [Add-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/Add-AzVirtualNetworkSubnetConfig) | 向虚拟网络添加子网配置。 |
| [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/Get-AzVirtualNetwork) | 获取资源组中的虚拟网络。 |
| [Set-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/Set-AzVirtualNetwork) | 设置虚拟网络的目标状态。 |
| [Get-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/Get-AzVirtualNetworkSubnetConfig) | 获取虚拟网络中的子网。 |
| [Set-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/Set-AzVirtualNetworkSubnetConfig) | 配置虚拟网络中子网配置的目标状态。 |
| [New-AzRouteTable](https://docs.microsoft.com/powershell/module/az.network/New-AzRouteTable) | 创建路由表。 |
| [Get-AzRouteTable](https://docs.microsoft.com/powershell/module/az.network/Get-AzRouteTable) | 获取路由表。 |
| [Set-AzRouteTable](https://docs.microsoft.com/powershell/module/az.network/Set-AzRouteTable) | 设置路由表的目标状态。 |
| [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/New-AzSqlInstance) | 创建托管实例。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

Azure SQL 托管实例的其他 PowerShell 脚本示例可在 [Azure SQL 托管实例 PowerShell 脚本](../../database/powershell-script-content-guide.md)中找到。
