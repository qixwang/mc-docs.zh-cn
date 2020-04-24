---
title: PowerShell 示例 - 故障转移组 - Azure SQL 数据库托管实例
description: Azure PowerShell 示例脚本，用于创建 Azure SQL 数据库托管实例，将其添加到故障转移组，然后测试故障转移。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 07/16/2019
ms.date: 12/16/2019
ms.openlocfilehash: 033c96334263443d2d300f46818e05ae3c4dea13
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75348503"
---
# <a name="use-powershell-to-add-an-azure-sql-database-managed-instance-to-a-failover-group"></a>使用 PowerShell 将 Azure SQL 数据库托管实例添加到故障转移组 

此 PowerShell 脚本示例创建两个托管实例，将其添加到故障转移组，然后测试从主托管实例到辅助托管实例的故障转移。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

本教程需要 AZ PowerShell 1.4.0 或更高版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。 此外，还需要运行 `Connect-AzAccount -Environment AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-scripts"></a>示例脚本

```azurepowershell
<#
Due to deployment times, you should plan for a full day to complete the entire script. 
You can monitor deployment progress in the activity log within the Azure portal.  

For more information on deployment times, see https://docs.azure.cn/sql-database/sql-database-managed-instance#managed-instance-management-operations. 

Closing the session will result in an incomplete deployment. To continue progress, you will
need to determine what the random modifier is and manually replace the random variable with 
the previously-assigned value. 
#>


# Connect-AzAccount -Environment AzureChinaCloud
# The SubscriptionId in which to create these objects
$SubscriptionId = '<Subscription-ID>'
# Create a random identifier to use as subscript for the different resource names
$randomIdentifier = $(Get-Random)
# Set the resource group name and location for your managed instance
$resourceGroupName = "myResourceGroup-$randomIdentifier"
$location = "chinaeast2"
$drLocation = "chinanorth2"

# Set the networking values for your primary managed instance
$primaryVNet = "primaryVNet-$randomIdentifier"
$primaryAddressPrefix = "10.0.0.0/16"
$primaryDefaultSubnet = "primaryDefaultSubnet-$randomIdentifier"
$primaryDefaultSubnetAddress = "10.0.0.0/24"
$primaryMiSubnetName = "primaryMISubnet-$randomIdentifier"
$primaryMiSubnetAddress = "10.0.0.0/24"
$primaryMiGwSubnetAddress = "10.0.255.0/27"
$primaryGWName = "primaryGateway-$randomIdentifier"
$primaryGWPublicIPAddress = $primaryGWName + "-ip"
$primaryGWIPConfig = $primaryGWName + "-ipc"
$primaryGWAsn = 61000
$primaryGWConnection = $primaryGWName + "-connection"


# Set the networking values for your secondary managed instance
$secondaryVNet = "secondaryVNet-$randomIdentifier"
$secondaryAddressPrefix = "10.128.0.0/16"
$secondaryDefaultSubnet = "secondaryDefaultSubnet-$randomIdentifier"
$secondaryDefaultSubnetAddress = "10.128.0.0/24"
$secondaryMiSubnetName = "secondaryMISubnet-$randomIdentifier"
$secondaryMiSubnetAddress = "10.128.0.0/24"
$secondaryMiGwSubnetAddress = "10.128.255.0/27"
$secondaryGWName = "secondaryGateway-$randomIdentifier"
$secondaryGWPublicIPAddress = $secondaryGWName + "-IP"
$secondaryGWIPConfig = $secondaryGWName + "-ipc"
$secondaryGWAsn = 62000
$secondaryGWConnection = $secondaryGWName + "-connection"



# Set the managed instance name for the new managed instances
$primaryInstance = "primary-mi-$randomIdentifier"
$secondaryInstance = "secondary-mi-$randomIdentifier"

# Set the admin login and password for your managed instance
$secpasswd = "PWD27!"+(New-Guid).Guid | ConvertTo-SecureString -AsPlainText -Force
$mycreds = New-Object System.Management.Automation.PSCredential ("azureuser", $secpasswd)


# Set the managed instance service tier, compute level, and license mode
$edition = "General Purpose"
$vCores = 8
$maxStorage = 256
$computeGeneration = "Gen5"
$license = "LicenseIncluded" #"BasePrice" or LicenseIncluded if you have don't have SQL Server licence that can be used for AHB discount

# Set failover group details
$vpnSharedKey = "mi1mi2psk"
$failoverGroupName = "failovergroup-$randomIdentifier"

# Show randomized variables
Write-host "Resource group name is" $resourceGroupName
Write-host "Password is" $secpasswd
Write-host "Primary Virtual Network name is" $primaryVNet
Write-host "Primary default subnet name is" $primaryDefaultSubnet
Write-host "Primary managed instance subnet name is" $primaryMiSubnetName
Write-host "Secondary Virtual Network name is" $secondaryVNet
Write-host "Secondary default subnet name is" $secondaryDefaultSubnet
Write-host "Secondary managed instance subnet name is" $secondaryMiSubnetName
Write-host "Primary managed instance name is" $primaryInstance
Write-host "Secondary managed instance name is" $secondaryInstance
Write-host "Failover group name is" $failoverGroupName

# Suppress networking breaking changes warning (https://aka.ms/azps-changewarnings
Set-Item Env:\SuppressAzurePowerShellBreakingChangeWarnings "true"

# Set subscription context
Set-AzContext -SubscriptionId $subscriptionId 

# Create a resource group
Write-host "Creating resource group..."
$resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location -Tag @{Owner="SQLDB-Samples"}
$resourceGroup

# Configure primary virtual network
Write-host "Creating primary virtual network..."
$primaryVirtualNetwork = New-AzVirtualNetwork `
                      -ResourceGroupName $resourceGroupName `
                      -Location $location `
                      -Name $primaryVNet `
                      -AddressPrefix $primaryAddressPrefix

                  Add-AzVirtualNetworkSubnetConfig `
                      -Name $primaryMiSubnetName `
                      -VirtualNetwork $primaryVirtualNetwork `
                      -AddressPrefix $PrimaryMiSubnetAddress `
                  | Set-AzVirtualNetwork
$primaryVirtualNetwork


# Configure primary MI subnet
Write-host "Configuring primary MI subnet..."
$primaryVirtualNetwork = Get-AzVirtualNetwork -Name $primaryVNet -ResourceGroupName $resourceGroupName


$primaryMiSubnetConfig = Get-AzVirtualNetworkSubnetConfig `
                        -Name $primaryMiSubnetName `
                        -VirtualNetwork $primaryVirtualNetwork
$primaryMiSubnetConfig

# Configure network security group management service
Write-host "Configuring primary MI subnet..."

$primaryMiSubnetConfigId = $primaryMiSubnetConfig.Id

$primaryNSGMiManagementService = New-AzNetworkSecurityGroup `
                      -Name 'primaryNSGMiManagementService' `
                      -ResourceGroupName $resourceGroupName `
                      -location $location
$primaryNSGMiManagementService

# Configure route table management service
Write-host "Configuring primary MI route table management service..."

$primaryRouteTableMiManagementService = New-AzRouteTable `
                      -Name 'primaryRouteTableMiManagementService' `
                      -ResourceGroupName $resourceGroupName `
                      -location $location
$primaryRouteTableMiManagementService

# Configure the primary network security group
Write-host "Configuring primary network security group..."
Set-AzVirtualNetworkSubnetConfig `
                      -VirtualNetwork $primaryVirtualNetwork `
                      -Name $primaryMiSubnetName `
                      -AddressPrefix $PrimaryMiSubnetAddress `
                      -NetworkSecurityGroup $primaryNSGMiManagementService `
                      -RouteTable $primaryRouteTableMiManagementService | `
                    Set-AzVirtualNetwork

Get-AzNetworkSecurityGroup `
                      -ResourceGroupName $resourceGroupName `
                      -Name "primaryNSGMiManagementService" `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 100 `
                      -Name "allow_management_inbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange 9000,9003,1438,1440,1452 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 200 `
                      -Name "allow_misubnet_inbound" `
                      -Access Allow `
                      -Protocol * `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix $PrimaryMiSubnetAddress `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 300 `
                      -Name "allow_health_probe_inbound" `
                      -Access Allow `
                      -Protocol * `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix AzureLoadBalancer `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1000 `
                      -Name "allow_tds_inbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 1433 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1100 `
                      -Name "allow_redirect_inbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 11000-11999 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1200 `
                      -Name "allow_geodr_inbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 5022 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 4096 `
                      -Name "deny_all_inbound" `
                      -Access Deny `
                      -Protocol * `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 100 `
                      -Name "allow_management_outbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange 80,443,12000 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 200 `
                      -Name "allow_misubnet_outbound" `
                      -Access Allow `
                      -Protocol * `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix $PrimaryMiSubnetAddress `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1100 `
                      -Name "allow_redirect_outbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 11000-11999 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1200 `
                      -Name "allow_geodr_outbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 5022 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 4096 `
                      -Name "deny_all_outbound" `
                      -Access Deny `
                      -Protocol * `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix * `
                    | Set-AzNetworkSecurityGroup
Write-host "Primary network security group configured successfully."


Get-AzRouteTable `
                      -ResourceGroupName $resourceGroupName `
                      -Name "primaryRouteTableMiManagementService" `
                    | Add-AzRouteConfig `
                      -Name "primaryToMIManagementService" `
                      -AddressPrefix 0.0.0.0/0 `
                      -NextHopType Internet `
                    | Add-AzRouteConfig `
                      -Name "ToLocalClusterNode" `
                      -AddressPrefix $PrimaryMiSubnetAddress `
                      -NextHopType VnetLocal `
                    | Set-AzRouteTable
Write-host "Primary network route table configured successfully."


# Create primary managed instance

Write-host "Creating primary managed instance..."
Write-host "This will take some time, see https://docs.azure.cn/sql-database/sql-database-managed-instance#managed-instance-management-operations for more information."
New-AzSqlInstance -Name $primaryInstance `
                      -ResourceGroupName $resourceGroupName `
                      -Location $location `
                      -SubnetId $primaryMiSubnetConfigId `
                      -AdministratorCredential $mycreds `
                      -StorageSizeInGB $maxStorage `
                      -VCore $vCores `
                      -Edition $edition `
                      -ComputeGeneration $computeGeneration `
                      -LicenseType $license
Write-host "Primary managed instance created successfully."

# Configure secondary virtual network
Write-host "Configuring secondary virtual network..."

$SecondaryVirtualNetwork = New-AzVirtualNetwork `
                      -ResourceGroupName $resourceGroupName `
                      -Location $drlocation `
                      -Name $secondaryVNet `
                      -AddressPrefix $secondaryAddressPrefix

Add-AzVirtualNetworkSubnetConfig `
                      -Name $secondaryMiSubnetName `
                      -VirtualNetwork $SecondaryVirtualNetwork `
                      -AddressPrefix $secondaryMiSubnetAddress `
                    | Set-AzVirtualNetwork
$SecondaryVirtualNetwork

# Configure secondary managed instance subnet
Write-host "Configuring secondary MI subnet..."

$SecondaryVirtualNetwork = Get-AzVirtualNetwork -Name $secondaryVNet -ResourceGroupName $resourceGroupName

$secondaryMiSubnetConfig = Get-AzVirtualNetworkSubnetConfig `
                        -Name $secondaryMiSubnetName `
                        -VirtualNetwork $SecondaryVirtualNetwork
$secondaryMiSubnetConfig

# Configure secondary network security group management service
Write-host "Configuring secondary network security group management service..."

$secondaryMiSubnetConfigId = $secondaryMiSubnetConfig.Id

$secondaryNSGMiManagementService = New-AzNetworkSecurityGroup `
                      -Name 'secondaryToMIManagementService' `
                      -ResourceGroupName $resourceGroupName `
                      -location $drlocation
$secondaryNSGMiManagementService

# Configure secondary route table MI management service
Write-host "Configuring secondary route table MI management service..."

$secondaryRouteTableMiManagementService = New-AzRouteTable `
                      -Name 'secondaryRouteTableMiManagementService' `
                      -ResourceGroupName $resourceGroupName `
                      -location $drlocation
$secondaryRouteTableMiManagementService

# Configure the secondary network security group
Write-host "Configuring secondary network security group..."

Set-AzVirtualNetworkSubnetConfig `
                      -VirtualNetwork $SecondaryVirtualNetwork `
                      -Name $secondaryMiSubnetName `
                      -AddressPrefix $secondaryMiSubnetAddress `
                      -NetworkSecurityGroup $secondaryNSGMiManagementService `
                      -RouteTable $secondaryRouteTableMiManagementService `
                    | Set-AzVirtualNetwork

Get-AzNetworkSecurityGroup `
                      -ResourceGroupName $resourceGroupName `
                      -Name "secondaryToMIManagementService" `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 100 `
                      -Name "allow_management_inbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange 9000,9003,1438,1440,1452 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 200 `
                      -Name "allow_misubnet_inbound" `
                      -Access Allow `
                      -Protocol * `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix $secondaryMiSubnetAddress `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 300 `
                      -Name "allow_health_probe_inbound" `
                      -Access Allow `
                      -Protocol * `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix AzureLoadBalancer `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1000 `
                      -Name "allow_tds_inbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 1433 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1100 `
                      -Name "allow_redirect_inbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 11000-11999 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1200 `
                      -Name "allow_geodr_inbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 5022 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 4096 `
                      -Name "deny_all_inbound" `
                      -Access Deny `
                      -Protocol * `
                      -Direction Inbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 100 `
                      -Name "allow_management_outbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange 80,443,12000 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 200 `
                      -Name "allow_misubnet_outbound" `
                      -Access Allow `
                      -Protocol * `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix $secondaryMiSubnetAddress `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1100 `
                      -Name "allow_redirect_outbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 11000-11999 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 1200 `
                      -Name "allow_geodr_outbound" `
                      -Access Allow `
                      -Protocol Tcp `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix VirtualNetwork `
                      -DestinationPortRange 5022 `
                      -DestinationAddressPrefix * `
                    | Add-AzNetworkSecurityRuleConfig `
                      -Priority 4096 `
                      -Name "deny_all_outbound" `
                      -Access Deny `
                      -Protocol * `
                      -Direction Outbound `
                      -SourcePortRange * `
                      -SourceAddressPrefix * `
                      -DestinationPortRange * `
                      -DestinationAddressPrefix * `
                    | Set-AzNetworkSecurityGroup


Get-AzRouteTable `
                      -ResourceGroupName $resourceGroupName `
                      -Name "secondaryRouteTableMiManagementService" `
                    | Add-AzRouteConfig `
                      -Name "secondaryToMIManagementService" `
                      -AddressPrefix 0.0.0.0/0 `
                      -NextHopType Internet `
                    | Add-AzRouteConfig `
                      -Name "ToLocalClusterNode" `
                      -AddressPrefix $secondaryMiSubnetAddress `
                      -NextHopType VnetLocal `
                    | Set-AzRouteTable
Write-host "Secondary network security group configured successfully."

# Create secondary managed instance

$primaryManagedInstanceId = Get-AzSqlInstance -Name $primaryInstance -ResourceGroupName $resourceGroupName | Select-Object Id


Write-host "Creating secondary managed instance..."
Write-host "This will take some time, see https://docs.azure.cn/sql-database/sql-database-managed-instance#managed-instance-management-operations for more information."
New-AzSqlInstance -Name $secondaryInstance `
                  -ResourceGroupName $resourceGroupName `
                  -Location $drLocation `
                  -SubnetId $secondaryMiSubnetConfigId `
                  -AdministratorCredential $mycreds `
                  -StorageSizeInGB $maxStorage `
                  -VCore $vCores `
                  -Edition $edition `
                  -ComputeGeneration $computeGeneration `
                  -LicenseType $license `
                  -DnsZonePartner $primaryManagedInstanceId.Id
Write-host "Secondary managed instance created successfully."


# Create primary gateway
Write-host "Adding GatewaySubnet to primary VNet..."
Get-AzVirtualNetwork `
                  -Name $primaryVNet `
                  -ResourceGroupName $resourceGroupName `
                | Add-AzVirtualNetworkSubnetConfig `
                  -Name "GatewaySubnet" `
                  -AddressPrefix $primaryMiGwSubnetAddress `
                | Set-AzVirtualNetwork

$primaryVirtualNetwork  = Get-AzVirtualNetwork `
                  -Name $primaryVNet `
                  -ResourceGroupName $resourceGroupName
$primaryGatewaySubnet = Get-AzVirtualNetworkSubnetConfig `
                  -Name "GatewaySubnet" `
                  -VirtualNetwork $primaryVirtualNetwork

Write-host "Creating primary gateway..."
Write-host "This will take some time."
$primaryGWPublicIP = New-AzPublicIpAddress -Name $primaryGWPublicIPAddress -ResourceGroupName $resourceGroupName `
         -Location $location -AllocationMethod Dynamic
$primaryGatewayIPConfig = New-AzVirtualNetworkGatewayIpConfig -Name $primaryGWIPConfig `
         -Subnet $primaryGatewaySubnet -PublicIpAddress $primaryGWPublicIP

$primaryGateway = New-AzVirtualNetworkGateway -Name $primaryGWName -ResourceGroupName $resourceGroupName `
    -Location $location -IpConfigurations $primaryGatewayIPConfig -GatewayType Vpn `
    -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $primaryGWAsn
$primaryGateway



# Create the secondary gateway
Write-host "Creating secondary gateway..."

Write-host "Adding GatewaySubnet to secondary VNet..."
Get-AzVirtualNetwork `
                  -Name $secondaryVNet `
                  -ResourceGroupName $resourceGroupName `
                | Add-AzVirtualNetworkSubnetConfig `
                  -Name "GatewaySubnet" `
                  -AddressPrefix $secondaryMiGwSubnetAddress `
                | Set-AzVirtualNetwork

$secondaryVirtualNetwork  = Get-AzVirtualNetwork `
                  -Name $secondaryVNet `
                  -ResourceGroupName $resourceGroupName
$secondaryGatewaySubnet = Get-AzVirtualNetworkSubnetConfig `
                  -Name "GatewaySubnet" `
                  -VirtualNetwork $secondaryVirtualNetwork
$drLocation = $secondaryVirtualNetwork.Location

Write-host "Creating primary gateway..."
Write-host "This will take some time."
$secondaryGWPublicIP = New-AzPublicIpAddress -Name $secondaryGWPublicIPAddress -ResourceGroupName $resourceGroupName `
         -Location $drLocation -AllocationMethod Dynamic
$secondaryGatewayIPConfig = New-AzVirtualNetworkGatewayIpConfig -Name $secondaryGWIPConfig `
         -Subnet $secondaryGatewaySubnet -PublicIpAddress $secondaryGWPublicIP

$secondaryGateway = New-AzVirtualNetworkGateway -Name $secondaryGWName -ResourceGroupName $resourceGroupName `
    -Location $drLocation -IpConfigurations $secondaryGatewayIPConfig -GatewayType Vpn `
    -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $secondaryGWAsn
$secondaryGateway


# Connect the primary to secondary gateway
Write-host "Connecting the primary gateway to secondary gateway..."
New-AzVirtualNetworkGatewayConnection -Name $primaryGWConnection -ResourceGroupName $resourceGroupName `
    -VirtualNetworkGateway1 $primaryGateway -VirtualNetworkGateway2 $secondaryGateway -Location $location `
    -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
$primaryGWConnection

# Connect the secondary to primary gateway
Write-host "Connecting the secondary gateway to primary gateway..."

New-AzVirtualNetworkGatewayConnection -Name $secondaryGWConnection -ResourceGroupName $resourceGroupName `
    -VirtualNetworkGateway1 $secondaryGateway -VirtualNetworkGateway2 $primaryGateway -Location $drLocation `
    -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
$secondaryGWConnection


# Create failover group
Write-host "Creating the failover group..."
$failoverGroup = New-AzSqlDatabaseInstanceFailoverGroup -Name $failoverGroupName `
     -Location $location -ResourceGroupName $resourceGroupName -PrimaryManagedInstanceName $primaryInstance `
     -PartnerRegion $drLocation -PartnerManagedInstanceName $secondaryInstance `
     -FailoverPolicy Automatic -GracePeriodWithDataLossHours 1
$failoverGroup

# Verify the current primary role
Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
    -Location $location -Name $failoverGroupName

# Failover the primary managed instance to the secondary role
Write-host "Failing primary over to the secondary location"
Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
    -Location $drLocation -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
Write-host "Successfully failed failover group to secondary location"

# Verify the current primary role
Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
    -Location $drLocation -Name $failoverGroupName

# Fail primary managed instance back to primary role
Write-host "Failing primary back to primary role"
Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
    -Location $location -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
Write-host "Successfully failed failover group to primary location"

# Verify the current primary role
Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $resourceGroupName `
    -Location $location -Name $failoverGroupName



# Clean up deployment 
<# You will need to remove the resource group twice. Removing the resource group the first time will remove the managed instance and virtual clusters but will then fail with the error message `Remove-AzResourceGroup : Long running operation failed with status 'Conflict'.`. Run the Remove-AzResourceGroup command a second time to remove any residual resources as well as the resource group. #> 

# Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
# Write-host "Removing managed instance and virtual cluster..."
# Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
# Write-host "Removing residual resources and resouce group..."


# Show randomized variables
Write-host "Resource group name is" $resourceGroupName
Write-host "Password is" $secpasswd
Write-host "Primary Virtual Network name is" $primaryVNet
Write-host "Primary default subnet name is" $primaryDefaultSubnet
Write-host "Primary managed instance subnet name is" $primaryMiSubnetName
Write-host "Secondary Virtual Network name is" $secondaryVNet
Write-host "Secondary default subnet name is" $secondaryDefaultSubnet
Write-host "Secondary managed instance subnet name is" $secondaryMiSubnetName
Write-host "Primary managed instance name is" $primaryInstance
Write-host "Secondary managed instance name is" $secondaryInstance
Write-host "Failover group name is" $failoverGroupName
```

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。 需要删除资源组两次。 第一次删除资源组将删除托管实例和虚拟群集，但随后会失败，并显示错误消息 `Remove-AzResourceGroup : Long running operation failed with status 'Conflict'.`。 再次运行 Remove-AzResourceGroup 命令，删除所有残留资源以及资源组。

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建 Azure 资源组。  |
| [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) | 创建虚拟网络。  |
| [Add-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | 向虚拟网络添加子网配置。 | 
| [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork) | 获取资源组中的虚拟网络。 | 
| [Get-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | 获取虚拟网络中的子网。 | 
| [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup) | 创建网络安全组。 | 
| [New-AzRouteTable](https://docs.microsoft.com/powershell/module/az.network/new-azroutetable) | 创建路由表。 |
| [Set-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | 更新虚拟网络的子网配置。  |
| [Set-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetwork) | 更新虚拟网络。  |
| [Get-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/get-aznetworksecuritygroup) | 获取网络安全组。 |
| [Add-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/add-aznetworksecurityruleconfig)| 将网络安全规则配置添加到网络安全组。 |
| [Set-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/set-aznetworksecuritygroup) | 更新网络安全组。  | 
| [Add-AzRouteConfig](https://docs.microsoft.com/powershell/module/az.network/add-azrouteconfig) | 将路由添加到路由表。 |
| [Set-AzRouteTable](https://docs.microsoft.com/powershell/module/az.network/set-azroutetable) | 更新路由表。  |
| [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance) | 创建 Azure SQL 数据库托管实例。  |
| [Get-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstance)| 返回有关 Azure SQL 托管数据库实例的信息。 |
| [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) | 创建公共 IP 地址。  | 
| [New-AzVirtualNetworkGatewayIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig) | 为虚拟网络网关创建 IP 配置 |
| [New-AzVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworkgateway) | 创建虚拟网络网关 |
| [New-AzVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworkgatewayconnection) | 创建两个虚拟网络网关之间的连接。   |
| [New-AzSqlDatabaseInstanceFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaseinstancefailovergroup)| 创建新的 Azure SQL 数据库托管实例故障转移组。  |
| [Get-AzSqlDatabaseInstanceFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaseinstancefailovergroup) | 获取或列出托管实例故障转移组。| 
| [Switch-AzSqlDatabaseInstanceFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/switch-azsqldatabaseinstancefailovergroup) | 执行托管实例故障转移组的故障转移。 | 
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组。 | 

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
