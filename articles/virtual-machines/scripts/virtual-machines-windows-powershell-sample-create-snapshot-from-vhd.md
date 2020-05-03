---
title: 使用 PowerShell 从 VHD 创建快照，在短时间内创建多个相同的托管磁盘
description: Azure PowerShell 脚本示例 - 从 VHD 创建快照以在短时间内创建多个相同的托管磁盘
services: virtual-machines-windows
documentationcenter: storage
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 06/05/2017
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: 2647b89f78d807fd682edf051b552301ab79710d
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596297"
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>使用 PowerShell 从 VHD 创建快照，在短时间内创建多个相同的托管磁盘

此脚本在相同或不同订阅的存储帐户中从 VHD 文件创建快照。 使用此脚本将专用 VHD（未通用化/未进行 sysprep）导入到某快照，然后使用该快照在短时间内创建多个相同的托管磁盘。 同时使用它将数据 VHD 导入到某快照，然后使用该快照在短时间内创建多个托管磁盘。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

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
$storageType = 'Standard_LRS'

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

## <a name="next-steps"></a>后续步骤

[从快照创建托管磁盘](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md)

[通过将托管磁盘附加为 OS 磁盘来创建虚拟机](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。

<!--Update_Description: update meta properties, update cmdlet -->