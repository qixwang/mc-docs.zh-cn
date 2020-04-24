---
title: 使用 CLI 基于快照创建托管磁盘
description: Azure CLI 脚本示例 - 基于快照创建托管磁盘
services: virtual-machines-windows
documentationcenter: storage
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 05/19/2017
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: f715ef2db954c81bc827f51991d7ed704485da50
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428153"
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a>使用 CLI 基于快照创建托管磁盘

此脚本基于快照创建托管磁盘。 可以使用它基于 OS 和数据磁盘的快照还原虚拟机。 基于相应的快照创建 OS 和数据托管磁盘，然后通过附加托管磁盘创建新的虚拟机。 还可以通过附加基于快照创建的数据磁盘来还原现有 VM 的数据磁盘。

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

#Provide the name of the new Managed Disks that will be create
diskName=myDiskName

#Provide the size of the disks in GB. It should be greater than the VHD file size.
diskSize=128

#Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
storageType=Premium_LRS

#Set the context to the subscription Id where Managed Disk will be created
az account set --subscription $subscriptionId

#Get the snapshot Id 
snapshotId=$(az snapshot show --name $snapshotName --resource-group $resourceGroupName --query [id] -o tsv)

#Create a new Managed Disks using the snapshot Id
#Note that managed disk will be created in the same location as the snapshot
az disk create --resource-group $resourceGroupName --name $diskName --sku $storageType --size-gb $diskSize --source $snapshotId

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令基于快照创建托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az snapshot show](https://docs.azure.cn/cli/snapshot?view=azure-cli-latest#az-snapshot-show) | 使用快照的名称和资源组属性获取快照的所有属性。 使用 ID 属性创建托管磁盘。  |
| [az disk create](https://docs.azure.cn/cli/disk?view=azure-cli-latest#az-disk-create) | 使用托管快照的快照 ID 创建托管磁盘 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Windows VM 文档](../windows/cli-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->