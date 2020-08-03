---
title: PowerShell 示例 - 使用 PowerShell 将托管磁盘复制到同一或不同订阅中
description: Azure PowerShell 脚本示例 - 将托管磁盘复制（或移动）到同一或不同订阅
services: virtual-machines-linux
documentationcenter: storage
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 06/06/2017
ms.date: 08/03/2020
ms.author: v-yeche
ms.openlocfilehash: 9127eb63e3cf4c4e0433b0550b799c23c79eb981
ms.sourcegitcommit: 692b9bad6d8e4d3a8e81c73c49c8cf921e1955e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2020
ms.locfileid: "87426357"
---
# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell"></a>在相同或不同订阅中通过 PowerShell 复制托管磁盘

此脚本在相同或不同订阅中创建现有托管磁盘的副本。 在父托管磁盘所在的区域中创建新磁盘。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Sign-in the Azure China Cloud
Connect-AzAccount -Environment AzureChinaCloud

#Provide the subscription Id of the subscription where managed disk exists
$sourceSubscriptionId='yourSourceSubscriptionId'

#Provide the name of your resource group where managed disk exists
$sourceResourceGroupName='mySourceResourceGroupName'

#Provide the name of the managed disk
$managedDiskName='myDiskName'

#Set the context to the subscription Id where Managed Disk exists
Select-AzSubscription -SubscriptionId $sourceSubscriptionId

#Get the source managed disk
$managedDisk= Get-AzDisk -ResourceGroupName $sourceResourceGroupName -DiskName $managedDiskName

#Provide the subscription Id of the subscription where managed disk will be copied to
#If managed disk is copied to the same subscription then you can skip this step
$targetSubscriptionId='yourTargetSubscriptionId'

#Name of the resource group where snapshot will be copied to
$targetResourceGroupName='myTargetResourceGroupName'

#Set the context to the subscription Id where managed disk will be copied to
#If snapshot is copied to the same subscription then you can skip this step
Select-AzSubscription -SubscriptionId $targetSubscriptionId

$diskConfig = New-AzDiskConfig -SourceResourceId $managedDisk.Id -Location $managedDisk.Location -CreateOption Copy 

#Create a new managed disk in the target subscription and resource group
New-AzDisk -Disk $diskConfig -DiskName $managedDiskName -ResourceGroupName $targetResourceGroupName

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令，通过源托管磁盘的 ID 在目标订阅中创建新的托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzDiskConfig) | 创建用于磁盘创建的磁盘配置。 包括父磁盘的资源 ID 以及与父磁盘位置相同的位置。  |
| [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | 使用磁盘配置、磁盘名称和作为参数传递的资源组名称创建磁盘。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Linux VM 文档](../linux/powershell-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->