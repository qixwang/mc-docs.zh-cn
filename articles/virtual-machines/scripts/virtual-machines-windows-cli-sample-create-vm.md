---
title: Azure CLI 脚本示例 - 创建 Windows Server VM | Azure
description: Azure CLI 脚本示例 - 创建 Windows Server VM
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
origin.date: 02/23/2017
ms.date: 08/12/2019
ms.author: v-yeche
ms.openlocfilehash: b90d4829ce9863535ed4fef404ebcb0cbe104e67
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69539187"
---
# <a name="create-a-virtual-machine-with-the-azure-cli"></a>使用 Azure CLI 创建虚拟机

该脚本将创建运行 Windows Server 2016 的 Azure 虚拟机。 运行脚本后，可通过远程桌面连接访问虚拟机。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Update for your admin password
AdminPassword=ChangeYourAdminPassword1

# Create a resource group.
az group create --name myResourceGroup --location chinanorth

# Create a virtual network.
az network vnet create --resource-group myResourceGroup --name myVnet --subnet-name mySubnet

# Create a public IP address.
az network public-ip create --resource-group myResourceGroup --name myPublicIP

# Create a network security group.
az network nsg create --resource-group myResourceGroup --name myNetworkSecurityGroup

# Create a virtual network card and associate with public IP address and NSG.
az network nic create \
  --resource-group myResourceGroup \
  --name myNic \
  --vnet-name myVnet \
  --subnet mySubnet \
  --network-security-group myNetworkSecurityGroup \
  --public-ip-address myPublicIP

# Create a virtual machine. 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location chinanorth \
    --nics myNic \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password $AdminPassword

# Open port 3389 to allow RDP traffic to host.
az vm open-port --port 3389 --resource-group myResourceGroup --name myVM

```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az network vnet create](https://docs.azure.cn/cli/network/vnet?view=azure-cli-latest#az-network-vnet-create) | 创建 Azure 虚拟网络和子网。 |
| [az network public-ip create](https://docs.azure.cn/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) | 使用静态 IP 地址和关联的 DNS 名称创建公共 IP 地址。 |
| [az network nsg create](https://docs.azure.cn/cli/network/nsg?view=azure-cli-latest#az-network-nsg-create) | 创建网络安全组 (NSG)，这是 Internet 和虚拟机之间的安全边界。 |
| [az network nic create](https://docs.azure.cn/cli/network/nic?view=azure-cli-latest#az-network-nic-create) | 创建虚拟网卡并将其连接到虚拟网络、子网和 NSG。 |
| [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和 NSG。 此命令还指定要使用的虚拟机映像和管理凭据。  |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Windows VM 文档](../windows/cli-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。

<!--Update_Description: update link-->