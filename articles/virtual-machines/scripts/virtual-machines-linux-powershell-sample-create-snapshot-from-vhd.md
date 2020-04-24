---
title: 使用 PowerShell 基于 VHD 创建快照以在短时间内创建多个相同的托管磁盘
description: Azure PowerShell 脚本示例 - 从 VHD 创建快照以在短时间内创建多个相同的托管磁盘
services: virtual-machines-linux
documentationcenter: storage
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 06/05/2017
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: 1f8b0a4ad4a4496a6ff82edb19418b7012a3f99c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428862"
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>使用 PowerShell 基于 VHD 创建快照以在短时间内创建多个相同的托管磁盘

此脚本基于 VHD 文件在同一或不同订阅中的存储帐户中创建快照。 可以使用此脚本将专用（非通用/系统准备的）VHD 导入到某个快照中，然后使用该快照在短时间内创建多个相同的托管磁盘。 另外，还可以使用它将数据 VHD 导入到某个快照中，然后使用该快照在短时间内创建多个托管磁盘。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<!--[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]-->

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Sign-in the Azure China Cloud
Connect-AzAccount -Environment AzureChinaCloud

#Provide the subscription Id where snapshot will be created
$subscriptionId = 'yourSubscriptionId'

#Provide the name of your resource group where snapshot will be created. 
$resourceGroupName ='yourResourceGroupName'

#Provide the name of the snapshot
$snapshotName = 'yourSnapshotName'

#Provide the storage type for snapshot. PremiumLRS or StandardLRS.
$storageType = 'StandardLRS'

#Provide the Azure region (e.g. chinanorth) where snapshot will be located.
#This location should be same as the storage account location where VHD file is stored 
#Get all the Azure location using command below:
#Get-AzLocation
$location = 'chinanorth'

#Provide the URI of the VHD file (page blob) in a storage account. Please not that this is NOT the SAS URI of the storage container where VHD file is stored. 
#e.g. https://contosostorageaccount1.blob.core.chinacloudapi.cn/vhds/contosovhd123.vhd
#Note: VHD file can be deleted as soon as Managed Disk is created.
$sourceVHDURI = 'https://yourStorageAccountName.blob.core.chinacloudapi.cn/vhds/yourVHDName.vhd'

#Provide the resource Id of the storage account where VHD file is stored. 
#e.g. /subscriptions/6582b1g7-e212-446b-b509-314e17e1efb0/resourceGroups/MDDemo/providers/Microsoft.Storage/storageAccounts/contosostorageaccount1
#This is an optional parameter if you are creating snapshot in the same subscription
$storageAccountId = '/subscriptions/yourSubscriptionId/resourceGroups/yourResourceGroupName/providers/Microsoft.Storage/storageAccounts/yourStorageAccountName'

#Set the context to the subscription Id where Managed Disk will be created
Select-AzSubscription -SubscriptionId $SubscriptionId

$snapshotConfig = New-AzSnapshotConfig -AccountType $storageType -Location $location -CreateOption Import -StorageAccountId $storageAccountId -SourceUri $sourceVHDURI 

New-AzSnapshot -Snapshot $snapshotConfig -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令基于某个 VHD 在不同订阅中创建托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzDiskConfig) | 创建用于创建磁盘的磁盘配置。 它包括存储着父 VHD 的存储帐户的存储类型、位置和资源 Id，以及父 VHD 的 VHD URI。 |
| [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | 使用作为参数传递的磁盘配置、磁盘名称和资源组名称创建磁盘。 |

## <a name="next-steps"></a>后续步骤

[基于快照创建托管磁盘](virtual-machines-linux-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Linux VM 文档](../linux/powershell-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。

<!-- Update_Description: update meta properties -->
