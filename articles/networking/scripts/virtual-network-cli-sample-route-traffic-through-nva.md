---
title: Azure CLI 脚本示例 - 通过网络虚拟设备路由流量 | Azure
description: Azure CLI 脚本示例 - 通过防火墙网络虚拟设备路由流量。
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 07/07/2017
ms.date: 12/09/2019
ms.author: v-tawe
ms.openlocfilehash: 0c6b89047390095f3dd7982d1d58c27e8c861018
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884065"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>通过网络虚拟设备路由流量

该脚本示例创建了包含前端和后端子网的虚拟网络。 它还会创建一个 VM，并启用 IP 转发，在两个子网之间路由流量。 运行脚本后，可将网络软件（例如防火墙应用程序）部署到 VM。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>示例脚本

```azurecli
# !/bin/bash

RgName="MyResourceGroup"
Location="chinaeast"

# Create a resource group.
az group create \
  --name $RgName \
  --location $Location

# Create a virtual network with a front-end subnet.
az network vnet create \
  --name MyVnet \
  --resource-group $RgName \
  --location $Location \
  --address-prefix 10.0.0.0/16 \
  --subnet-name MySubnet-FrontEnd \
  --subnet-prefix 10.0.1.0/24

# Create a network security group for the front-end subnet allowing HTTP and HTTPS inbound.
az network nsg create \
  --resource-group $RgName \
  --name MyNsg-FrontEnd \
  --location $Location

# Create NSG rules to allow HTTP & HTTPS traffic inbound.
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-FrontEnd \
  --name Allow-HTTP-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "<em>" \
  --destination-address-prefix "</em>" \
  --destination-port-range 80
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-FrontEnd \
  --name Allow-HTTPS-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix Internet \
  --source-port-range "<em>" \
  --destination-address-prefix "</em>" \
  --destination-port-range 443

# Associate the front-end NSG to the front-end subnet.
az network vnet subnet update \
  --vnet-name MyVnet \
  --name MySubnet-FrontEnd \
  --resource-group $RgName \
  --network-security-group MyNsg-FrontEnd

# Create the back-end subnet.
az network vnet subnet create \
  --address-prefix 10.0.2.0/24 \
  --name MySubnet-BackEnd \
  --resource-group $RgName \
  --vnet-name MyVnet

# Create the DMZ subnet.
az network vnet subnet create \
  --address-prefix 10.0.0.0/24 \
  --name MySubnet-Dmz \
  --resource-group $RgName \
  --vnet-name MyVnet

# Create a public IP address for the firewall VM.
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIP-Firewall

# Create a NIC for the firewall VM and enable IP forwarding.
az network nic create \
  --resource-group $RgName \
  --name MyNic-Firewall \
  --vnet-name MyVnet \
  --subnet MySubnet-Dmz \
  --public-ip-address MyPublicIp-Firewall \
  --ip-forwarding

# Create a firewall VM to accept all traffic between the front and back-end subnets.
az vm create \
  --resource-group $RgName \
  --name MyVm-Firewall \
  --nics MyNic-Firewall \
  --image UbuntuLTS \
  --admin-username azureadmin \
  --generate-ssh-keys

# Get the private IP address from the VM for the user-defined route.
Fw1Ip=$(az vm list-ip-addresses \
  --resource-group $RgName \
  --name MyVm-Firewall \
  --query [].virtualMachine.network.privateIpAddresses[0] --out tsv)

# Create route table for the FrontEnd subnet.
az network route-table create \
  --name MyRouteTable-FrontEnd \
  --resource-group $RgName

# Create a route for traffic from the front-end to the back-end subnet through the firewall VM.
az network route-table route create \
  --name RouteToBackEnd \
  --resource-group $RgName \
  --route-table-name MyRouteTable-FrontEnd \
  --address-prefix 10.0.2.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $Fw1Ip
  
# Create a route for traffic from the front-end subnet to the Internet through the firewall VM.
az network route-table route create \
  --name RouteToInternet \
  --resource-group $RgName \
  --route-table-name MyRouteTable-FrontEnd \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $Fw1Ip

# Associate the route table to the FrontEnd subnet.
az network vnet subnet update \
  --name MySubnet-FrontEnd \
  --vnet-name MyVnet \
  --resource-group $RgName \
  --route-table MyRouteTable-FrontEnd

# Create route table for the BackEnd subnet.
az network route-table create \
  --name MyRouteTable-BackEnd \
  --resource-group $RgName
  
# Create a route for traffic from the back-end subnet to the front-end subnet through the firewall VM.
az network route-table route create \
  --name RouteToFrontEnd \
  --resource-group $RgName \
  --route-table-name MyRouteTable-BackEnd \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $Fw1Ip

# Create a route for traffic from the back-end subnet to the Internet through the firewall VM.
az network route-table route create \
  --name RouteToInternet \
  --resource-group $RgName \
  --route-table-name MyRouteTable-BackEnd \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $Fw1Ip

# Associate the route table to the BackEnd subnet.
az network vnet subnet update \
  --name MySubnet-BackEnd \
  --vnet-name MyVnet \
  --resource-group $RgName \
  --route-table MyRouteTable-BackEnd
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络和网络安全组。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](/cli/group) | 创建用于存储所有资源的资源组。 |
| [az network vnet create](/cli/network/vnet) | 创建 Azure 虚拟网络和前端子网。 |
| [az network subnet create](/cli/network/vnet/subnet) | 创建后端子网和 DMZ 子网。 |
| [az network public-ip create](/cli/network/public-ip) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [az network nic create](/cli/network/nic) | 创建虚拟网络接口，并对它启用 IP 转发。 |
| [az network nsg create](/cli/network/nsg) | 创建网络安全组 (NSG)。 |
| [az network nsg rule create](/cli/network/nsg/rule) | 创建允许 HTTP 和 HTTPS 端口入站到 VM 的 NSG 规则。 |
| [az network vnet subnet update](/cli/network/vnet/subnet)| 将 NSG 和路由表关联到子网。 |
| [az network route-table create](/cli/network/route-table#az-network-route-table-create)| 为所有路由创建路由表。 |
| [az network route-table route create](/cli/network/route-table/route#az-network-route-table-route-create)| 创建路由，通过 VM 在子网和 Internet 之间路由流量。 |
| [az vm create](/cli/vm) | 创建虚拟机并向其附加 NIC。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [az group delete](/cli/group) | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可在 [Azure 网络概述文档](../cli-samples.md)中找到其他网络 CLI 脚本示例