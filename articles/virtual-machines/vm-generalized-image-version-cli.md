---
title: 使用 Azure CLI 从通用化映像创建 VM
description: 使用 Azure CLI 从通用化映像版本创建 VM。
author: rockboyfor
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
origin.date: 05/04/2020
ms.date: 07/06/2020
ms.author: v-yeche
ms.openlocfilehash: 8a375c1b648d2351e2e0715ac2582b91cbf9de64
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945607"
---
<!--Verified successfully-->
# <a name="create-a-vm-from-a-generalized-image-version-using-the-cli"></a>使用 CLI 从通用化映像版本创建 VM

从[共享映像库](/virtual-machines/linux/shared-image-galleries#generalized-and-specialized-images)中存储的通用化映像版本创建 VM。 若要使用专用化映像创建 VM，请参阅[从专用化映像创建 VM](vm-specialized-image-version-powershell.md)。 

## <a name="get-the-image-id"></a>获取映像 ID

使用 [az sig image-definition list](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-list) 列出库中的映像定义，以查看定义的名称和 ID。

```azurecli 
resourceGroup=myGalleryRG
gallery=myGallery
az sig image-definition list --resource-group $resourceGroup --gallery-name $gallery --query "[].[name, id]" --output tsv
```

## <a name="create-the-vm"></a>创建 VM

运行 [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) 创建 VM。 若要使用最新版本的映像，请将 `--image` 设置为映像定义的 ID。 

在此示例中，请根据需要替换资源名称。 

```azurecli 
imgDef="/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition"
vmResourceGroup=myResourceGroup
location=chinaeast
vmName=myVM
adminUsername=azureuser

az group create --name $vmResourceGroup --location $location

az vm create\
   --resource-group $vmResourceGroup \
   --name $vmName \
   --image $imgDef \
   --admin-username $adminUsername \
   --generate-ssh-keys
```

也可以通过使用 `--image` 参数的映像版本 ID 来使用特定版本。 例如，若要使用映像版本 *1.0.0*，请键入：`--image "/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0"`。

<!--Not Available on ## Next steps-->

<!--Not Available on [Azure Image Builder (preview)](./windows/image-builder-overview.md)-->
<!--Not Available on [create a new image version from an existing image version](./windows/image-builder-gallery-update-image-version.md)-->

<!-- Update_Description: update meta properties, wording update, update link -->