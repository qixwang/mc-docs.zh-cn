---
title: Azure CLI 脚本示例 - 使用 DSC 创建具有 IIS 的 VM
description: Azure CLI 脚本示例 - 使用 DSC 创建具有 IIS 的 Windows Server 2016 Datacenter
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 02/23/2017
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 9b6bf0a126247d4b5d3afe73c38692ecef0fe292
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596405"
---
# <a name="create-a-vm-with-iis-using-dsc"></a>使用 DSC 创建具有 IIS 的 VM

此脚本创建一个虚拟机，并使用 Azure 虚拟机 DSC 自定义脚本扩展安装和配置 IIS。 

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

# Create a VM
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password $AdminPassword

# Start a CustomScript extension to use a simple bash script to update, download and install WordPress and MySQL 
az vm extension set \
   --name DSC \
   --publisher Microsoft.Powershell \
   --version 2.19 \
   --vm-name myVM \
   --resource-group myResourceGroup \
   --settings '{"ModulesURL":"https://github.com/Azure/azure-quickstart-templates/raw/master/dsc-extension-iis-server-windows-vm/ContosoWebsite.ps1.zip", "configurationFunction": "ContosoWebsite.ps1\\ContosoWebsite", "Properties": {"MachineName": "myVM"} }'

  # open port 80 to allow web traffic to host
  az vm open-port \
    --port 80 \
    --resource-group myResourceGroup \
    --name myVM

```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和 NSG。 此命令还指定要使用的虚拟机映像和管理凭据。  |
| [az vm extension set](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-extension-set) | 将自定义脚本扩展添加到虚拟机，此扩展将调用脚本来安装 IIS。 |
| [az vm open-port](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-open-port) | 创建网络安全组规则，以允许入站流量。 在此示例中，为 HTTP 流量打开端口 80。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

<!--CORRECT ON [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete)-->

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Windows VM 文档](../windows/cli-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。

<!--Update_Description: update link-->