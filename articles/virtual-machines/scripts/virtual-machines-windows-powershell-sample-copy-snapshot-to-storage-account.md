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
ms.openlocfilehash: c9532339568f6483729d5afad5b35af40a6b797b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428556"
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-powershell"></a>使用 PowerShell 将托管快照作为 VHD 导出/复制到不同区域中的存储帐户

此脚本将托管快照导出到不同区域中的存储帐户。 它首先生成快照的 SAS URI，然后使用该 SAS URI 将快照复制到不同区域中的存储帐户。 可以使用此脚本在不同区域中维护托管磁盘的副本以备用于灾难恢复。  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>示例脚本

<!--CORRECT LINE 50 ON https://docs.azure.cn/storage/storage-dotnet-shared-access-signature-part-1-->

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
#Know more about SAS here: https://docs.azure.cn/storage/storage-dotnet-shared-access-signature-part-1
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
$sas = Grant-AzSnapshotAccess -ResourceGroupName $ResourceGroupName -SnapshotName $SnapshotName  -DurationInSecond $sasExpiryDuration -Access Read

#Create the context for the storage account which will be used to copy snapshot to the storage account 
$destinationContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

#Copy the snapshot to the storage account 
Start-AzStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer $storageContainerName -DestContext $destinationContext -DestBlob $destinationVHDFileName

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令生成托管快照的 SAS URI 并使用该 SAS URI 将快照复制到一个存储帐户。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [Grant-AzSnapshotAccess](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | 生成快照的 SAS URI，该 SAS URI 用来将快照复制到存储帐户。 |
| [New-AzStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/New-AzStorageContext) | 使用帐户名称和密钥创建存储帐户上下文。 可以使用此上下文对存储帐户执行读取/写入操作。 |
| [Start-AzStorageBlobCopy](https://docs.microsoft.com/powershell/module/azure.storage/Start-AzStorageBlobCopy) | 将快照的基础 VHD 复制到存储帐户 |

## <a name="next-steps"></a>后续步骤

[基于 VHD 创建托管磁盘](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md)

[基于托管磁盘创建虚拟机](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。

<!--Update_Description: update meta properties, update link-->
