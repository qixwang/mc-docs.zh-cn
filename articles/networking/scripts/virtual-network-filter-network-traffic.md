---
title: Azure CLI 脚本示例 - 筛选 VM 网络流量 | Microsoft Docs
description: Azure CLI 脚本示例 - 筛选入站和出站 VM 网络流量。
services: virtual-network
documentationcenter: virtual-network
author: KumudD
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 07/07/2017
ms.dat: 12/09/2017
ms.author: v-tawe
ms.openlocfilehash: 5fe2cc04d27b475ed26513a21d87b4840e8664c2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884064"
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>筛选入站和出站 VM 网络流量

该脚本示例创建了包含前端和后端子网的虚拟网络。 前端子网的入站网络流量仅限于 HTTP、HTTPS 和 SSH，而从后端子网到 Internet 的出站流量则不受限制。 运行该脚本后，将具有一个包含两个 NIC 的虚拟机。 每个 NIC 连接到不同的子网。

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

# Create a virtual network and a front-end subnet.
az network vnet create \
  --resource-group $RgName \
  --name MyVnet \
  --address-prefix 10.0.0.0/16  \
  --location $Location \
  --subnet-name MySubnet-FrontEnd \
  --subnet-prefix 10.0.1.0/24

# Create a back-end subnet.
az network vnet subnet create \
  --address-prefix 10.0.2.0/24 \
  --name MySubnet-BackEnd \
  --resource-group $RgName \
  --vnet-name MyVnet

# Create a network security group (NSG) for the front-end subnet.
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

# Create an NSG rule to allow SSH traffic in from the Internet to the front-end subnet.
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-FrontEnd \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix Internet \
  --source-port-range "<em>" \
  --destination-address-prefix "</em>" \
  --destination-port-range 22

# Associate the front-end NSG to the front-end subnet.
az network vnet subnet update \
  --vnet-name MyVnet \
  --name MySubnet-FrontEnd \
  --resource-group $RgName \
  --network-security-group MyNsg-FrontEnd

# Create a network security group for the back-end subnet.
az network nsg create \
  --resource-group $RgName \
  --name MyNsg-BackEnd \
  --location $Location

# Create an NSG rule to block all outbound traffic from the back-end subnet to the Internet (inbound blocked by default).
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-BackEnd \
  --name Deny-Internet-All \
  --access Deny --protocol Tcp \
  --direction Outbound --priority 100 \
  --source-address-prefix "<em>" \
  --source-port-range "</em>" \
  --destination-address-prefix "Internet" \
  --destination-port-range "*"

# Associate the back-end NSG to the back-end subnet.
az network vnet subnet update \
  --vnet-name MyVnet \
  --name MySubnet-BackEnd \
  --resource-group $RgName \
  --network-security-group MyNsg-BackEnd

# Create a public IP address for the VM front-end network interface.
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIp-FrontEnd \
  --allocation-method Dynamic

# Create a network interface for the VM attached to the front-end subnet.
az network nic create \
  --resource-group $RgName \
  --vnet-name MyVnet \
  --subnet MySubnet-FrontEnd \
  --name MyNic-FrontEnd \
  --public-ip-address MyPublicIp-FrontEnd

# Create a network interface for the VM attached to the back-end subnet.
az network nic create \
  --resource-group $RgName \
  --vnet-name MyVnet \
  --subnet MySubnet-BackEnd \
  --name MyNic-BackEnd

# Create the VM with both the FrontEnd and BackEnd NICs.
az vm create \
  --resource-group $RgName \
  --name MyVm \
  --nics MyNic-FrontEnd MyNic-BackEnd \
  --image UbuntuLTS \
  --admin-username azureadmin \
  --generate-ssh-keys
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
| [az network subnet create](/cli/network/vnet/subnet) | 创建后端子网。 |
| [az network vnet subnet update](/cli/network/vnet/subnet) | 将 NSG 关联到子网。 |
| [az network public-ip create](/cli/network/public-ip) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [az network nic create](/cli/network/nic) | 创建虚拟网络接口，并将其附加到虚拟网络的前端和后端子网。 |
| [az network nsg create](/cli/network/nsg) | 创建关联到前端和后端子网的网络安全组 (NSG)。 |
| [az network nsg rule create](/cli/network/nsg/rule) |创建 NSG 规则，允许或阻止特定子网的特定端口。 |
| [az vm create](/cli/vm) | 创建虚拟机，并将 NIC 附加到每个 VM。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [az group delete](/cli/group) | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

可在 [Azure 网络概述文档](../cli-samples.md)中找到其他网络 CLI 脚本示例