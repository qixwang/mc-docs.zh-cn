---
title: 使用 CLI 将快照导出/复制到不同区域中的存储帐户
description: Azure CLI 脚本示例 - 将快照作为 VHD 导出/复制到相同或不同区域中的存储帐户。
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
ms.custom: mvc,seodec18
ms.openlocfilehash: dc0ce571b667681f1c2cd0de7108a3339d16decb
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428898"
---
# <a name="exportcopy-a-snapshot-to-a-storage-account-in-different-region-with-cli"></a>使用 CLI 将快照导出/复制到不同区域中的存储帐户

此脚本将托管快照导出到不同区域中的存储帐户。 它首先生成快照的 SAS URI，然后使用该 SAS URI 将快照复制到不同区域中的存储帐户。 可以使用此脚本在不同区域中维护托管磁盘的副本以备用于灾难恢复。 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#Provide the subscription Id where snapshot is created
subscriptionId=dd80b94e-0463-4a65-8d04-c94f403879dc

#Provide the name of your resource group where snapshot is created
resourceGroupName=myResourceGroupName

#Provide the snapshot name 
snapshotName=mySnapshotName

#Provide Shared Access Signature (SAS) expiry duration in seconds e.g. 3600.
#Know more about SAS here: https://docs.azure.cn/storage/storage-dotnet-shared-access-signature-part-1
sasExpiryDuration=3600

#Provide storage account name where you want to copy the snapshot. 
storageAccountName=mystorageaccountname

#Name of the storage container where the downloaded snapshot will be stored
storageContainerName=mystoragecontainername

#Provide the key of the storage account where you want to copy snapshot. 
storageAccountKey=mystorageaccountkey

#Provide the name of the VHD file to which snapshot will be copied.
destinationVHDFileName=myvhdfilename

az account set --subscription $subscriptionId

sas=$(az snapshot grant-access --resource-group $resourceGroupName --name $snapshotName --duration-in-seconds $sasExpiryDuration --query [accessSas] -o tsv)

az storage blob copy start --destination-blob $destinationVHDFileName --destination-container $storageContainerName --account-name $storageAccountName --account-key $storageAccountKey --source-uri $sas

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令生成托管快照的 SAS URI 并使用该 SAS URI 将快照复制到一个存储帐户。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az snapshot grant-access](https://docs.azure.cn/cli/snapshot?view=azure-cli-latest#az-snapshot-grant-access) | 生成只读 SAS，使用该 SAS 可以将基础 VHD 文件复制到存储帐户或将其下载到本地  |
| [az storage blob copy start](https://docs.azure.cn/cli/storage/blob/copy?view=azure-cli-latest#az-storage-blob-copy-start) | 将 blob 从一个存储帐户异步复制到另一个存储帐户 |

## <a name="next-steps"></a>后续步骤

[基于 VHD 创建托管磁盘](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[基于托管磁盘创建虚拟机](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。

<!--Update_Description: update meta properties, update links -->