---
title: 使用 CLI 从快照创建虚拟机
description: Azure CLI 脚本示例 - 从快照创建 VM
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
ms.openlocfilehash: 099d42231b8c0731e6ce94520055cc1d9ed238f2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428721"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>使用 CLI 从快照创建虚拟机

此脚本从 OS 磁盘的快照创建虚拟机。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#Provide the subscription Id of the subscription where you want to create Managed Disks
subscriptionId=dd80b94e-0463-4a65-8d04-c94f403879dc

#Provide the name of your resource group
resourceGroupName=myResourceGroupName

#Provide the name of the snapshot that will be used to create Managed Disks
snapshotName=mySnapshotName

#Provide the name of the Managed Disk
osDiskName=myOSDiskName

#Provide the size of the disks in GB. It should be greater than the VHD file size.
diskSize=128

#Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
storageType=Premium_LRS

#Provide the OS type
osType=linux

#Provide the name of the virtual machine
virtualMachineName=myVirtualMachineName

#Set the context to the subscription Id where Managed Disk will be created
az account set --subscription $subscriptionId

#Get the snapshot Id 
snapshotId=$(az snapshot show --name $snapshotName --resource-group $resourceGroupName --query [id] -o tsv)

#Create a new Managed Disks using the snapshot Id
az disk create --resource-group $resourceGroupName --name $osDiskName --sku $storageType --size-gb $diskSize --source $snapshotId 

#Create VM by attaching created managed disks as OS
az vm create --name $virtualMachineName --resource-group $resourceGroupName --attach-os-disk $osDiskName --os-type $osType

```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建托管磁盘、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az snapshot show](https://docs.azure.cn/cli/snapshot?view=azure-cli-latest#az-snapshot-show) | 使用快照名称和资源组名称获取快照。 返回对象的 ID 属性用于创建托管磁盘。  |
| [az disk create](https://docs.azure.cn/cli/disk?view=azure-cli-latest#az-disk-create) | 使用快照 ID、磁盘名称、存储类型和大小从快照创建托管磁盘  |
| [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) | 使用托管 OS 磁盘创建 VM |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。

<!--Update_Description: update meta properties -->