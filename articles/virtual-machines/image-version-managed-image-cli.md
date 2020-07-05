---
title: 使用 Azure CLI 从托管映像迁移到映像版本
description: 了解如何使用 Azure CLI 从托管映像迁移到共享映像库中的映像版本。
author: rockboyfor
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
origin.date: 05/04/2020
ms.date: 07/06/2020
ms.author: v-yeche
ms.reviewer: akjosh
ms.openlocfilehash: 6d3a9ebc98d885541ff5ef1b2558f20ffebdbe87
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945996"
---
# <a name="migrate-from-a-managed-image-to-an-image-version-using-the-azure-cli"></a>使用 Azure CLI 从托管映像迁移到映像版本
如果打算将现有托管映像迁移到共享映像库，可以直接从托管映像创建共享映像库映像。 测试新映像后，可以删除源托管映像。 还可以使用 [PowerShell](image-version-managed-image-powershell.md) 从托管映像迁移到共享映像库。

映像库中的映像具有两个组件，我们将在此示例中创建这两个组件：
- “映像定义”包含有关映像及其使用要求的信息。 这包括该映像是 Windows 映像还是 Linux 映像、是专用映像还是通用映像，此外还包括发行说明以及最低和最高内存要求。 它是某种映像类型的定义。 
- 使用共享映像库时，将使用映像版本来创建 VM。 可根据环境的需要创建多个映像版本。 创建 VM 时，将使用该映像版本为 VM 创建新磁盘。 可以多次使用映像版本。

## <a name="before-you-begin"></a>准备阶段

若要完成本文，必须具有现有的[共享映像库](shared-images-cli.md)。 

若要完成本文中的示例，必须具有通用化 VM 的现有托管映像。 有关详细信息，请参阅[捕获托管映像](./linux/capture-image.md)。 如果托管映像包含数据磁盘，则数据磁盘大小不能超过 1 TB。

通过本文进行操作时，请根据需要替换资源组和 VM 名称。

## <a name="create-an-image-definition"></a>创建映像定义

由于托管映像始终是通用化映像，因此你将使用通用化映像的 `--os-state generalized` 创建映像定义。

映像定义名称可能包含大写或小写字母、数字、点、短划线和句点。 

若要详细了解可为映像定义指定的值，请参阅[映像定义](/virtual-machines/linux/shared-image-galleries#image-definitions)。

使用 [az sig image-definition create](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-create) 在库中创建一个映像定义。

在此示例中，映像定义名为 myImageDefinition，适用于[通用化](./linux/shared-image-galleries.md#generalized-and-specialized-images) Linux OS 映像。 若要使用 Windows OS 创建映像的定义，请使用 `--os-type Windows`。 

```azurecli 
resourceGroup=myGalleryRG
gallery=myGallery
imageDef=myImageDefinition
az sig image-definition create \
   --resource-group $resourceGroup \
   --gallery-name $gallery \
   --gallery-image-definition $imageDef \
   --publisher myPublisher \
   --offer myOffer \
   --sku mySKU \
   --os-type Linux \
   --os-state generalized
```

## <a name="create-the-image-version"></a>创建映像版本

使用 [az image gallery create-image-version](https://docs.microsoft.com/cli/azure/sig/image-version?view=azure-cli-latest#az-sig-image-version-create) 创建版本。 你需要传入托管映像的 ID 以作为创建映像版本时要使用的基线。 可以使用 [az image list](https://docs.azure.cn/cli/image?view?view=azure-cli-latest#az-image-list) 获取映像的 ID。 

```azurecli
az image list --query "[].[name, id]" -o tsv
```

允许用于映像版本的字符为数字和句点。 数字必须在 32 位整数范围内。 格式：*MajorVersion*.*MinorVersion*.*Patch*。

在此示例中，映像的版本为 *1.0.0*，我们打算使用区域冗余存储在“中国东部”区域创建 1 个副本，在“中国东部 2”区域创建 1 个副本。 选择复制的目标区域时，请记住，你还需包括源区域作为复制的目标。

在 `--managed-image` 参数中传递托管映像的 ID。

```azurecli 
imageID="/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
az sig image-version create \
   --resource-group $resourceGroup \
   --gallery-name $gallery \
   --gallery-image-definition $imageDef \
   --gallery-image-version 1.0.0 \
   --target-regions "chinaeast=1" "chinaeast2=1=premium_lrs" \
   --replica-count 2 \
   --managed-image $imageID
```

> [!NOTE]
> 需等待映像版本彻底生成并复制完毕，然后才能使用同一托管映像来创建另一映像版本。
>
> 也可在高级冗余存储中存储所有映像版本副本，只需在创建映像版本时添加 `--storage-account-type premium_lrs` 即可。
>

## <a name="next-steps"></a>后续步骤

从[通用化映像版本](vm-generalized-image-version-cli.md)创建 VM。

<!-- Update_Description: new article about image version managed image cli -->
<!--NEW.date: 06/01/2020-->