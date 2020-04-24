---
title: 通过 CLI 使用现有托管 OS 磁盘创建虚拟机
description: Azure CLI 脚本示例 - 通过将托管磁盘附加为 OS 磁盘创建 VM
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 05/10/2017
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: c8629893ccd6e5b410b859adafee097bc00c68dd
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428896"
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a>通过 CLI 使用现有托管 OS 磁盘创建虚拟机

此脚本通过将现有托管磁盘附加为 OS 磁盘来创建虚拟机。 在前面的方案中使用此脚本：
* 基于从不同订阅中的托管磁盘复制的现有托管 OS 磁盘创建 VM
* 基于从专用 VHD 文件创建的现有托管磁盘创建 VM 
* 基于从快照创建的现有托管 OS 磁盘创建 VM 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#Provide the subscription Id
subscriptionId=6492b1f7-f219-446b-b509-314e17e1efb0

#Provide the name of your resource group
resourceGroupName=myResourceGroupName

#Provide the name of the Managed Disk
managedDiskName=myDiskName

#Provide the OS type
osType=linux

#Provide the name of the virtual machine
virtualMachineName=myVirtualMachineName123

#Set the context to the subscription Id where Managed Disk exists and where VM will be created
az account set --subscription $subscriptionId

#Get the resource Id of the managed disk
managedDiskId=$(az disk show --name $managedDiskName --resource-group $resourceGroupName --query [id] -o tsv)

#Create VM by attaching existing managed disks as OS
az vm create --name $virtualMachineName --resource-group $resourceGroupName --attach-os-disk $managedDiskId --os-type $osType

```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令获取托管磁盘属性，将托管磁盘附加到新 VM 并创建 VM。 表中的每一项均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az disk show](https://docs.azure.cn/cli/disk?view=azure-cli-latest#az-disk-show) | 使用磁盘名称和资源组名称获取托管磁盘属性。 Id 属性用来将托管磁盘附加到新 VM |
| [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) | 使用托管 OS 磁盘创建 VM |
## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。

<!--Update_Description: update meta properties, update cmdlet -->