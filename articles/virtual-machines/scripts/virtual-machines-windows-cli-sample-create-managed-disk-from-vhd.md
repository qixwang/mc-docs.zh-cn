---
title: 使用 CLI 基于相同订阅的存储帐户中的 VHD 文件创建托管磁盘
description: Azure CLI 脚本示例 - 基于相同订阅的存储帐户中的 VHD 文件创建托管磁盘
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
ms.openlocfilehash: aa4a917f9079ac72cdf6ca7faeaf41c240c5d3e4
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428331"
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a>使用 CLI 基于相同订阅的存储帐户中的 VHD 文件创建托管磁盘

此脚本基于相同订阅的存储帐户中的 VHD 文件创建托管磁盘。 使用此脚本将专用（未经过通用化/sysprep 处理）的 VHD 导入到托管 OS 磁盘以创建虚拟机。 或者使用它将数据 VHD 导入到托管数据磁盘。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#Provide the subscription Id
subscriptionId=mySubscriptionId

#Provide the name of your resource group.
#Ensure that resource group is already created 
resourceGroupName=myResourceGroupName

#Provide the name of the Managed Disk
diskName=myDiskName

#Provide the size of the disks in GB. It should be greater than the VHD file size.
diskSize=128

#Provide the URI of the VHD file that will be used to create Managed Disk. 
# VHD file can be deleted as soon as Managed Disk is created.
# e.g. https://contosostorageaccount1.blob.core.chinacloudapi.cn/vhds/contosovhd123.vhd 
vhdUri=https://contosostorageaccount1.blob.core.chinacloudapi.cn/vhds/contosoumd78620170425131836.vhd

#Provide the storage type for the Managed Disk. Premium_LRS or Standard_LRS.
storageType=Premium_LRS

#Provide the Azure location (e.g. chinanorth) where Managed Disk will be located. 
#The location should be same as the location of the storage account where VHD file is stored.
#Get all the Azure location supported for your subscription using command below:
#az account list-locations
location=chinanorth

#Set the context to the subscription Id where Managed Disk will be created
az account set --subscription $subscriptionId

#Create the Managed disk from the VHD file 
az disk create --resource-group $resourceGroupName --name $diskName --sku $storageType --location $location --size-gb $diskSize --source $vhdUri

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令基于 VHD 创建托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az disk create](https://docs.azure.cn/cli/disk?view=azure-cli-latest#az-disk-create) | 使用相同订阅的存储帐户中的 VHD 的 URI 创建托管磁盘 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Windows VM 文档](../windows/cli-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->