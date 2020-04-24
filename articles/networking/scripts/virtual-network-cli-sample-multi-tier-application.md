---
title: Azure CLI 脚本示例 - 为多层应用程序创建网络 | Azure
description: Azure CLI 脚本示例 - 为多层应用程序创建虚拟网络。
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 05/16/2017
ms.date: 12/09/2019
ms.author: v-tawe
ms.openlocfilehash: cda247d2e7822f98e94ae730fe48529781900294
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74883978"
---
# <a name="create-a-network-for-multi-tier-applications"></a>为多层应用程序创建网络

该脚本示例创建了包含前端和后端子网的虚拟网络。 传入前端子网的流量仅限 HTTP 和 SSH，而传入后端子网的流量限于 MySQL、端口 3306。 运行该脚本后，将具有两个虚拟机（在可向其中部署 Web 服务器和 MySQL 软件的每个子网中各具有一个虚拟机）。

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

# Create a back-end subnet.
az network vnet subnet create \
  --address-prefix 10.0.2.0/24 \
  --name MySubnet-BackEnd \
  --resource-group $RgName \
  --vnet-name MyVnet

# Create a network security group for the front-end subnet.
az network nsg create \
  --resource-group $RgName \
  --name MyNsg-FrontEnd \
  --location $Location

# Create an NSG rule to allow HTTP traffic in from the Internet to the front-end subnet.
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

# Create a network security group for back-end subnet.
az network nsg create \
  --resource-group $RgName \
  --name MyNsg-BackEnd \
  --location $Location

# Create an NSG rule to allow MySQL traffic from the front-end subnet to the back-end subnet.
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-BackEnd \
  --name Allow-MySql-FrontEnd \
  --access Allow --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "<em>" \
  --destination-address-prefix "</em>" \
  --destination-port-range 3306

# Create an NSG rule to allow SSH traffic from the Internet to the front-end subnet.
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-BackEnd \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix Internet \
  --source-port-range "<em>" \
  --destination-address-prefix "</em>" \
  --destination-port-range 22

# Create an NSG rule to block all outbound traffic from the back-end subnet to the Internet (NOTE: If you run the MySQL installation below this rule will be disabled and then re-enabled).
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-BackEnd \
  --name Deny-Internet-All \
  --access Deny --protocol Tcp \
  --direction Outbound --priority 300 \
  --source-address-prefix "<em>" \
  --source-port-range "</em>" \
  --destination-address-prefix "<em>" \
  --destination-port-range "</em>"

# Associate the back-end NSG to the back-end subnet.
az network vnet subnet update \
  --vnet-name MyVnet \
  --name MySubnet-BackEnd \
  --resource-group $RgName \
  --network-security-group MyNsg-BackEnd

# Create a public IP address for the web server VM.
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIP-Web

# Create a NIC for the web server VM.
az network nic create \
  --resource-group $RgName \
  --name MyNic-Web \
  --vnet-name MyVnet \
  --subnet MySubnet-FrontEnd \
  --network-security-group MyNsg-FrontEnd \
  --public-ip-address MyPublicIP-Web

# Create a Web Server VM in the front-end subnet.
az vm create \
  --resource-group $RgName \
  --name MyVm-Web \
  --nics MyNic-Web \
  --image UbuntuLTS \
  --admin-username azureadmin \
  --generate-ssh-keys

# Create a public IP address for the MySQL VM.
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIP-Sql

# Create a NIC for the MySQL VM.
az network nic create \
  --resource-group $RgName \
  --name MyNic-Sql \
  --vnet-name MyVnet \
  --subnet MySubnet-BackEnd \
  --network-security-group MyNsg-BackEnd \
  --public-ip-address MyPublicIP-Sql

# Create a MySQL VM in the back-end subnet.
az vm create \
  --resource-group $RgName \
  --name MyVm-Sql \
  --nics MyNic-Sql \
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
| [az network public-ip create](/cli/network/public-ip) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [az network nic create](/cli/network/nic) | 创建虚拟网络接口，并将其附加到虚拟网络的前端和后端子网。 |
| [az network nsg create](/cli/network/nsg) | 创建关联到前端和后端子网的网络安全组 (NSG)。 |
| [az network nsg rule create](/cli/network/nsg/rule) |创建 NSG 规则，允许或阻止特定子网的特定端口。 |
| [az vm create](/cli/vm) | 创建虚拟机，并将 NIC 附加到每个 VM。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [az group delete](/cli/group) | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可在 [Azure 网络概述文档](../cli-samples.md)中找到其他网络 CLI 脚本示例