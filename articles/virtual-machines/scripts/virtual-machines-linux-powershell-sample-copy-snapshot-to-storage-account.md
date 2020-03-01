---
title: 使用 PowerShell 将托管快照作为 VHD 导出/复制到不同区域中的存储帐户
description: Azure PowerShell 脚本示例 - 将快照作为 VHD 导出/复制到不同区域中的存储帐户
services: virtual-machines-windows
documentationcenter: storage
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 06/05/2017
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: f5068ba375097bdd3817b4c28796912f03bdb006
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428351"
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-powershell"></a>使用 PowerShell 将托管快照作为 VHD 导出/复制到不同区域中的存储帐户

此脚本将托管快照导出到不同区域中的存储帐户。 它首先会生成快照的 SAS URI，然后使用它将快照复制到不同区域中的存储帐户。 使用此脚本将托管磁盘的备份保留在灾难恢复的不同区域。  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Sign-in the Azure China Cloud
Connect-AzAccount -Environment AzureChinaCloud

#Provide the subscription Id of the subscription where snapshot is created
$subscriptionId = "yourSubscriptionId"

#Provide the name of your resource group where snapshot is created
$resourceGroupName ="yourResourceGroupName"

#Provide the snapshot name 
$snapshotName = "yourSnapshotName"

#Provide Shared Access Signature (SAS) expiry duration in seconds e.g. 3600.
#Know more about SAS here: https://docs.microsoft.com/Az.Storage/storage-dotnet-shared-access-signature-part-1
$sasExpiryDuration = "3600"

#Provide storage account name where you want to copy the snapshot. 
$storageAccountName = "yourstorageaccountName"

#Name of the storage container where the downloaded snapshot will be stored
$storageContainerName = "yourstoragecontainername"

#Provide the key of the storage account where you want to copy snapshot. 
$storageAccountKey = 'yourStorageAccountKey'

#Provide the name of the VHD file to which snapshot will be copied.
$destinationVHDFileName = "yourvhdfilename"

# Set the context to the subscription Id where Snapshot is created
Select-AzSubscription -SubscriptionId $SubscriptionId

#Generate the SAS for the snapshot 
$sas = Grant-AzSnapshotAccess -ResourceGroupName $ResourceGroupName -SnapshotName $SnapshotName -DurationInSecond $sasExpiryDuration -Access Read
#Create the context for the storage account which will be used to copy snapshot to the storage account 
$destinationContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

#Copy the snapshot to the storage account 
Start-AzStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer $storageContainerName -DestContext $destinationContext -DestBlob $destinationVHDFileName

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令为托管快照生成 SAS URI，并使用 SAS URI 将快照复制到存储帐户。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Grant-AzSnapshotAccess](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | 生成快照的 SAS URI，用于将快照复制到存储帐户。 |
| [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/New-AzureStorageContext) | 使用帐户名和密钥创建存储帐户上下文。 此上下文可用于对存储帐户执行读/写操作。 |
| [Start-AzureStorageBlobCopy](https://docs.microsoft.com/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | 将快照的基础 VHD 复制到存储帐户 |

## <a name="next-steps"></a>后续步骤

[从 VHD 创建托管磁盘](virtual-machines-linux-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)

[从托管磁盘创建虚拟机](./virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Linux VM 文档](../linux/powershell-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。

<!-- Update_Description: update meta properties -->
