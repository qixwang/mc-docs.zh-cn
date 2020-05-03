---
title: 使用 CLI 将快照导出/复制到不同区域中的存储帐户
description: Azure CLI 脚本示例 - 将快照作为 VHD 导出/复制到相同或不同区域中的存储帐户。
services: virtual-machines-windows
documentationcenter: storage
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 05/19/2017
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: mvc,seodec18
ms.openlocfilehash: f34ec07e455584dc1e4d6188a461b6095b0bca17
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596368"
---
# <a name="exportcopy-a-snapshot-to-a-storage-account-in-different-region-with-cli"></a>使用 CLI 将快照导出/复制到不同区域中的存储帐户

此脚本将托管快照导出到不同区域中的存储帐户。 它首先会生成快照的 SAS URI，然后使用它将快照复制到不同区域中的存储帐户。 使用此脚本将托管磁盘的备份保留在灾难恢复的不同区域。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#Provide the subscription Id where snapshot is created
subscriptionId=<mySubscriptionId>

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

此脚本使用以下命令为托管快照生成 SAS URI，并使用 SAS URI 将快照复制到存储帐户。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az snapshot grant-access](https://docs.azure.cn/cli/snapshot?view=azure-cli-latest#az-snapshot-grant-access) | 生成只读 SAS，用于将基础 VHD 文件复制到存储帐户或将其下载到本地  |
| [az storage blob copy start](https://docs.azure.cn/cli/storage/blob/copy?view=azure-cli-latest#az-storage-blob-copy-start) | 以异步方式将 blob 从一个存储帐户复制到另一个存储帐户 |

## <a name="next-steps"></a>后续步骤

[从 VHD 创建托管磁盘](virtual-machines-windows-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Windows VM 文档](../windows/cli-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->