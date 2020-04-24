---
title: 使用 CLI 将托管磁盘复制到相同或不同的订阅 | Azure
description: Azure CLI 脚本示例 - 将托管磁盘复制（或移动）到同一或不同订阅
services: virtual-machines-linux
documentationcenter: storage
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 05/19/2017
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 7a093e8361af54a6ca5ca7200135123519ac1959
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428899"
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>使用 CLI 将托管磁盘复制（移动）到相同或不同订阅

此脚本会将托管磁盘复制到相同订阅或相同区域中的不同订阅。 仅当订阅属于同一 AAD 租户时，复制才适用。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#Provide the subscription Id of the subscription where managed disk exists
sourceSubscriptionId=dd80b94e-0463-4a65-8d04-c94f403879dc

#Provide the name of your resource group where managed disk exists
sourceResourceGroupName=mySourceResourceGroupName

#Provide the name of the managed disk
managedDiskName=myDiskName

#Set the context to the subscription Id where managed disk exists
az account set --subscription $sourceSubscriptionId

#Get the managed disk Id 
managedDiskId=$(az disk show --name $managedDiskName --resource-group $sourceResourceGroupName --query [id] -o tsv)

#If managedDiskId is blank then it means that managed disk does not exist.
echo 'source managed disk Id is: ' $managedDiskId

#Provide the subscription Id of the subscription where managed disk will be copied to
targetSubscriptionId=6492b1f7-f219-446b-b509-314e17e1efb0

#Name of the resource group where managed disk will be copied to
targetResourceGroupName=mytargetResourceGroupName

#Set the context to the subscription Id where managed disk will be copied to
az account set --subscription $targetSubscriptionId

#Copy managed disk to different subscription using managed disk Id
az disk create --resource-group $targetResourceGroupName --name $managedDiskName --source $managedDiskId

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令并使用源托管磁盘的 Id 在目标订阅中创建新托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az disk show](https://docs.azure.cn/cli/disk?view=azure-cli-latest#az-disk-show) | 使用托管磁盘的名称和资源组属性获取托管磁盘的所有属性。 Id 属性用于将托管磁盘复制到其他订阅。  |
| [az disk create](https://docs.azure.cn/cli/disk?view=azure-cli-latest#az-disk-create) | 使用父托管磁盘的 ID 和名称在其他订阅中新建托管磁盘，从而复制托管磁盘。  |

## <a name="next-steps"></a>后续步骤

[基于托管磁盘创建虚拟机](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md)

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->