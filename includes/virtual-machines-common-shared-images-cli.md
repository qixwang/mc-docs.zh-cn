---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 05/21/2019
ms.date: 07/01/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 3b33738848ffd2d6a0f5d8b290043df4490afbf2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77029262"
---
## <a name="before-you-begin"></a>准备阶段

若要完成本文中的示例，必须具有通用化 VM 的现有托管映像。 有关详细信息，请参阅[教程：使用 Azure CLI 创建 Azure VM 的自定义映像](/virtual-machines/linux/tutorial-custom-images)。 如果托管映像包含数据磁盘，则数据磁盘大小不能超过 1 TB。

## <a name="launch-azure-local-cli"></a>启动 Azure 本地 CLI


如果希望在本地安装和使用 CLI，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

## <a name="create-an-image-gallery"></a>创建映像库 

映像库是用于启用映像共享的主要资源。 允许用于库名称的字符为大写或小写字母、数字、点和句点。 库名称不能包含短划线。   库名称在你的订阅中必须唯一。 

使用 [az sig create](https://docs.azure.cn/cli/sig?view=azure-cli-latest#az-sig-create) 创建一个映像库。 以下示例在 *myGalleryRG* 中创建一个名为 *myGallery* 的库。

```azurecli
az group create --name myGalleryRG --location chinanorth
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>创建映像定义

映像定义为映像创建一个逻辑分组。 它们用于管理有关映像版本的信息，这些版本是在其中创建的。 映像定义名称可能包含大写或小写字母、数字、点、短划线和句点。 若要详细了解可以为映像定义指定的值，请参阅[映像定义](/virtual-machines/linux/shared-image-galleries#image-definitions)。

使用 [az sig image-definition create](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-create) 在该库中创建一个初始映像定义。

```azurecli 
az sig image-definition create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku 16.04-LTS \
   --os-type Linux 
```

## <a name="create-an-image-version"></a>创建映像版本 

使用 [az image gallery create-image-version](https://docs.microsoft.com/cli/azure/sig/image-version?view=azure-cli-latest#az-sig-image-version-create) 根据需要创建映像的版本。 你需要传入托管映像的 ID 以作为创建映像版本时要使用的基线。 可以使用 [az image list](https://docs.azure.cn/cli/image?view?view=azure-cli-latest#az-image-list) 获取资源组中的映像的相关信息。 

允许用于映像版本的字符为数字和句点。 数字必须在 32 位整数范围内。 格式：*MajorVersion*.*MinorVersion*.*Patch*。

在此示例中，映像的版本为 *1.0.0*，并且我们打算在“中国北部”  区域创建 2 个副本，在“中国东部区域”  创建 1 个副本，在“中国东部 2”区域创建 1 个副本。 

<!--Not Avaialble on zone-redundant storage-->

```azurecli 
az sig image-version create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "chinanorth" "chinaeast=1" "chinaeast2=1" \
   --replica-count 2 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

> [!NOTE]
> 需等待映像版本彻底生成并复制完毕，然后才能使用同一托管映像来创建另一映像版本。
>

<!--Not Available on > You can also store your image version in Zone Redundant Storage by adding `--storage-account-type standard_zrs` when you create the image version.-->
<!--Not Avaialble on [Zone Redundant Storage](/storage/common/storage-redundancy-zrs)-->

## <a name="share-the-gallery"></a>共享库

我们建议你在库级别与其他用户共享。 若要获取库的对象 ID，请使用 [az sig show](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-show)。

```azurecli
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

使用对象 ID 作为作用域以及电子邮件地址，并使用 [az role assignment create](https://docs.azure.cn/cli/role/assignment?view=azure-cli-latest#az-role-assignment-create) 授予用户对共享映像库的访问权限。

```azurecli
az role assignment create --role "Reader" --assignee <email address> --scope <gallery ID>
```