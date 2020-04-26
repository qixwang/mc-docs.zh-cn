---
title: CLI 示例 - 故障转移组 - Azure SQL 数据库托管实例
description: Azure CLI 示例脚本，用于创建 Azure SQL 数据库托管实例，将其添加到故障转移组，然后测试故障转移。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 07/16/2019
ms.date: 04/27/2020
ms.openlocfilehash: db7dc7e646cb441a1c701c9cce76e3d045d6a64f
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127237"
---
# <a name="use-cli-to-add-an-azure-sql-database-managed-instance-to-a-failover-group"></a>使用 CLI 将 Azure SQL 数据库托管实例添加到故障转移组

此 Azure CLI 示例创建两个托管实例，将其添加到故障转移组，然后测试从主托管实例到辅助托管实例的故障转移。

本文要求运行 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-scripts"></a>示例脚本

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>运行脚本

```azurecli
#!/bin/bash
# Due to deployment times, you should plan for a full day to complete the entire script. You can monitor deployment progress in the activity log within the Azure portal. For more information on deployment times, see https://docs.azure.cn/sql-database/sql-database-managed-instance#managed-instance-management-operations. 

location="China East 2"
failoverLocation="China North 2"
randomIdentifier=random123

resource="resource-$randomIdentifier"
failoverResource="failoverResource-$randomIdentifier"

vnet="vnet-$randomIdentifier"
subnet="subnet-$randomIdentifier"
nsg="nsg-$randomIdentifier"
route="route-$randomIdentifier"
instance="instance-$randomIdentifier"

failover="failover-$randomIdentifier"

failoverVnet="failoverVnet-$randomIdentifier"
failoverSubnet="failoverSubnet-$randomIdentifier"
failoverNsg="failoverNsg-$randomIdentifier"
failoverRoute="failoverRoute-$randomIdentifier"
failoverInstance="failoverInstance-$randomIdentifier"

login="sampleLogin"
password="samplePassword123!"

vpnSharedKey="abc123"

gateway="gateway-$randomIdentifier"
gatewayIP="$gateway-ip"
gatewayConnection="$gateway-connection"

$failoverGateway="failoverGateway-$randomIdentifier"
$failoverGatewayIP="$failoverGateway-ip"
$failoverGatewayConnection="$failoverGateway-connection"

echo "Using resource group $resource and $failoverResource with login: $login, password: $password..."

echo "Creating $resource..."
az group create --name $resource --location "$location"
az group create --name $failoverResource --location "$failoverLocation"

echo "Creating $vnet with $subnet..."
az network vnet create --name $vnet --resource-group $resource --location "$location" --address-prefixes 10.0.0.0/16
az network vnet subnet create --name $subnet --resource-group $resource --vnet-name $vnet --address-prefixes 10.0.0.0/24

echo "Creating $nsg..."
az network nsg create --name $nsg --resource-group $resource --location "$location"

az network nsg rule create --name "allow_management_inbound" --nsg-name $nsg --priority 100 --resource-group $resource --access Allow --destination-address-prefixes 10.0.0.0/24 --destination-port-ranges 9000 9003 1438 1440 1452 --direction Inbound --protocol Tcp --source-address-prefixes "*" --source-port-ranges "*"
az network nsg rule create --name "allow_misubnet_inbound" --nsg-name $nsg --priority 200 --resource-group $resource --access Allow --destination-address-prefixes 10.0.0.0/24 --destination-port-ranges "*" --direction Inbound --protocol "*" --source-address-prefixes 10.0.0.0/24 --source-port-ranges "*"
az network nsg rule create --name "allow_health_probe_inbound" --nsg-name $nsg --priority 300 --resource-group $resource --access Allow --destination-address-prefixes 10.0.0.0/24 --destination-port-ranges "*" --direction Inbound --protocol "*" --source-address-prefixes AzureLoadBalancer --source-port-ranges "*"
az network nsg rule create --name "allow_management_outbound" --nsg-name $nsg --priority 1100 --resource-group $resource --access Allow --destination-address-prefixes AzureCloud --destination-port-ranges 443 12000 --direction Outbound --protocol Tcp --source-address-prefixes 10.0.0.0/24 --source-port-ranges "*"
az network nsg rule create --name "allow_misubnet_outbound" --nsg-name $nsg --priority 200 --resource-group $resource --access Allow --destination-address-prefixes 10.0.0.0/24 --destination-port-ranges "*" --direction Outbound --protocol "*" --source-address-prefixes 10.0.0.0/24 --source-port-ranges "*"

echo "Creating $route..."
az network route-table create --name $route --resource-group $resource --location "$location"

az network route-table route create --address-prefix 0.0.0.0/0 --name "primaryToMIManagementService" --next-hop-type Internet --resource-group $resource --route-table-name $route
az network route-table route create --address-prefix 10.0.0.0/24 --name "ToLocalClusterNode" --next-hop-type VnetLocal --resource-group $resource --route-table-name $route

echo "Configuring $subnet with $nsg and $route..."
az network vnet subnet update --name $subnet --network-security-group $nsg --route-table $route --vnet-name $vnet --resource-group $resource 

echo "Creating $($instance) with $vnet and $subnet..."
az sql mi create --admin-password $password --admin-user $login --name $instance --resource-group $resource --subnet $subnet --vnet-name $vnet --location "$location" --assign-identity

echo "Creating $failoverVnet with $failoverSubnet..."
az network vnet create --name $failoverVnet --resource-group $failoverResource --location "$failoverLocation" --address-prefixes 10.128.0.0/16
az network vnet subnet create --name $failoverSubnet --resource-group $failoverResource --vnet-name $failoverVnet --address-prefixes 10.128.0.0/24

echo "Creating $failoverNsg..."
az network nsg create --name $failoverNsg --resource-group $failoverResource --location "$failoverLocation"

az network nsg rule create --name "allow_management_inbound" --nsg-name $failoverNsg --priority 100 --resource-group $failoverResource --access Allow --destination-address-prefixes 10.128.0.0/24 --destination-port-ranges 9000 9003 1438 1440 1452 --direction Inbound --protocol Tcp --source-address-prefixes "*" --source-port-ranges "*"
az network nsg rule create --name "allow_misubnet_inbound" --nsg-name $failoverNsg --priority 200 --resource-group $failoverResource --access Allow --destination-address-prefixes 10.128.0.0/24 --destination-port-ranges "*" --direction Inbound --protocol "*" --source-address-prefixes 10.128.0.0/24 --source-port-ranges "*"
az network nsg rule create --name "allow_health_probe_inbound" --nsg-name $failoverNsg --priority 300 --resource-group $failoverResource --access Allow --destination-address-prefixes 10.128.0.0/24 --destination-port-ranges "*" --direction Inbound --protocol "*" --source-address-prefixes AzureLoadBalancer --source-port-ranges "*"
az network nsg rule create --name "allow_management_outbound" --nsg-name $failoverNsg --priority 1100 --resource-group $failoverResource --access Allow --destination-address-prefixes AzureCloud --destination-port-ranges 443 12000 --direction Outbound --protocol Tcp --source-address-prefixes 10.128.0.0/24 --source-port-ranges "*"
az network nsg rule create --name "allow_misubnet_outbound" --nsg-name $failoverNsg --priority 200 --resource-group $failoverResource --access Allow --destination-address-prefixes 10.128.0.0/24 --destination-port-ranges "*" --direction Outbound --protocol "*" --source-address-prefixes 10.128.0.0/24 --source-port-ranges "*"

echo "Creating $failoverRoute..."
az network route-table create --name $failoverRoute --resource-group $failoverResource --location "$failoverLocation"

az network route-table route create --address-prefix 0.0.0.0/0 --name "primaryToMIManagementService" --next-hop-type Internet --resource-group $failoverResource --route-table-name $failoverRoute
az network route-table route create --address-prefix 10.128.0.0/24 --name "ToLocalClusterNode" --next-hop-type VnetLocal --resource-group $failoverResource --route-table-name $failoverRoute

echo "Configuring $failoverSubnet with $failoverNsg and $failoverRoute..."
az network vnet subnet update --name $failoverSubnet --network-security-group $failoverNsg --route-table $failoverRoute --vnet-name $failoverVnet --resource-group $failoverResource 

echo "Creating $failoverInstance with $failoverVnet and $failoverSubnet..."
az sql mi create --admin-password $password --admin-user $login --name $failoverInstance --resource-group $failoverResource --subnet $failoverSubnet --vnet-name $failoverVnet --location "$failoverLocation" --assign-identity

echo "Creating gateway..."
az network vnet subnet create --name "GatewaySubnet" --resource-group $resource --vnet-name $vnet --address-prefixes 10.0.255.0/27
az network public-ip create --name $gatewayIP --resource-group $resource --allocation-method Dynamic --location "$location"
az network vnet-gateway create --name $gateway --public-ip-addresses $gatewayIP --resource-group $resource --vnet $vnet --asn 61000 --gateway-type Vpn --location "$location" --sku VpnGw1 --vpn-type RouteBased #-EnableBgp $true

echo "Creating failover gateway..."
az network vnet subnet create --name "GatewaySubnet" --resource-group $failoverResource --vnet-name $failoverVnet --address-prefixes 10.128.255.0/27
az network public-ip create --name $failoverGatewayIP --resource-group $failoverResource --allocation-method Dynamic --location "$failoverLocation"
az network vnet-gateway create --name $failoverGateway --public-ip-addresses $failoverGatewayIP --resource-group $failoverResource --vnet $failoverVnet --asn 62000 --gateway-type Vpn --location "$failoverLocation" --sku VpnGw1 --vpn-type RouteBased

echo "Connecting gateway and failover gateway..."
az network vpn-connection create --name $gatewayConnection --resource-group $resource --vnet-gateway1 $gateway --enable-bgp --location "$location" --vnet-gateway2 $failoverGateway --shared-key $vpnSharedKey
az network vpn-connection create --name $failoverGatewayConnection --resource-group $failoverResource --vnet-gateway1 $failoverGateway --enable-bgp --location "$failoverLocation" --shared-key $vpnSharedKey --vnet-gateway2 $gateway

echo "Creating the failover group..."
az sql instance-failover-group create --mi $instance --name $failover --partner-mi $failoverInstance --resource-group $resource --partner-resource-group $failoverResource --failover-policy Automatic --grace-period 1 
az sql instance-failover-group show --location "$location" --name $failover --resource-group $resource # verify the primary role

echo "Failing managed instance over to secondary location..."
az sql instance-failover-group set-primary --location "$failoverLocation" --name $failover --resource-group $resource
az sql instance-failover-group show --location "$failoverLocation" --name $failover --resource-group $resource # verify the primary role

echo "Failing managed instance back to primary location..."
az sql instance-failover-group set-primary --location "$location" --name $failover --resource-group $resource
az sql instance-failover-group show --location "$location" --name $failover --resource-group $resource # verify the primary role
```

### <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。 需要删除资源组两次。 第一次删除资源组将删除托管实例和虚拟群集，但随后会失败，并显示错误消息 `az group delete : Long running operation failed with status 'Conflict'.`。 再次运行 az group delete 命令，删除所有残留资源以及资源组。

```azurecli
az group delete --name $resource
```

## <a name="sample-reference"></a>示例参考

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| | |
|---|---|
| [az network vnet](/cli/network/vnet) | 虚拟网络命令。  |
| [az network vnet subnet](/cli/network/vnet/subnet) | 虚拟网络子网命令。 |
| [az network nsg](/cli/network/nsg) | 网络安全组命令。 |
| [az network nsg rule](/cli/network/nsg/rule)| 网络安全规则命令。 |
| [az network route-table](/cli/network/route-table) | 路由表命令。 |
| [az sql mi](/cli/sql/mi) | 托管实例命令。 |
| [az network public-ip](/cli/network/public-ip) | 网络公共 IP 地址命令。 |
| [az network vnet-gateway](/cli/network/vnet-gateway) | 虚拟网络网关命令 |
| [az sql instance-failover-group](/cli/sql/instance-failover-group) | 托管实例故障转移组命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
