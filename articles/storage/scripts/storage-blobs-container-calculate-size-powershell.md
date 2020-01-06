---
title: 使用 PowerShell 计算 blob 容器的大小
titleSuffix: Azure Storage
description: 通过计算容器各 blob 的总大小来计算 Azure Blob 存储中容器的大小。
services: storage
author: WenJason
ms.custom: mvc
ms.service: storage
ms.topic: sample
origin.date: 12/04/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.openlocfilehash: fd8af0b57b3ba149a1bd5a101724ef31d9ad6c5e
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624315"
---
# <a name="calculate-the-size-of-a-blob-container-with-powershell"></a>使用 PowerShell 计算 blob 容器的大小

此脚本通过计算容器中 blob 的总大小来计算 Azure Blob 存储中容器的大小。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> 此 PowerShell 脚本提供容器的估计大小，不应该用于计费计算。 有关为计费目的计算容器大小的脚本，请参阅[为计费目的计算 Blob 存储容器的大小](../scripts/storage-blobs-container-calculate-billing-size-powershell.md)。 

## <a name="sample-script"></a>示例脚本

```Powershell
# this script will show how to get the total size of the blobs in a container
# before running this, you need to create a storage account, create a container,
#    and upload some blobs into the container 
# note: this retrieves all of the blobs in the container in one command. 
#       if you are going to run this against a container with a lot of blobs
#       (more than a couple hundred), use continuation tokens to retrieve
#       the list of blobs.

# these are for the storage account to be used
$resourceGroup = "bloblisttestrg"
$storageAccountName = "contosobloblisttest"
$containerName = "listtestblobs"

# get a reference to the storage account and the context
$storageAccount = Get-AzStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName
$ctx = $storageAccount.Context 

# get a list of all of the blobs in the container 
$listOfBLobs = Get-AzStorageBlob -Container $ContainerName -Context $ctx 

# zero out our total
$length = 0

# this loops through the list of blobs and retrieves the length for each blob
#   and adds it to the total
$listOfBlobs | ForEach-Object {$length = $length + $_.Length}

# output the blobs and their sizes and the total 
Write-Host "List of Blobs and their size (length)"
Write-Host " " 
$listOfBlobs | select Name, Length
Write-Host " "
Write-Host "Total Length = " $length
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、容器和所有相关资源。

```powershell
Remove-AzResourceGroup -Name bloblisttestrg
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令来计算 Blob 存储容器的大小。 表中的每一项均链接到命令特定的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/get-azstorageaccount) | 获取资源组或订阅中的指定存储帐户或所有存储帐户。 |
| [Get-AzStorageBlob](https://docs.microsoft.com/powershell/module/az.storage/Get-AzStorageBlob) | 列出容器中的 Blob。 |

## <a name="next-steps"></a>后续步骤

有关为计费目的计算容器大小的脚本，请参阅[为计费目的计算 Blob 存储容器的大小](../scripts/storage-blobs-container-calculate-billing-size-powershell.md)。

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure 存储的 PowerShell 示例](../blobs/storage-samples-blobs-powershell.md)中找到其他存储 PowerShell 脚本示例。
