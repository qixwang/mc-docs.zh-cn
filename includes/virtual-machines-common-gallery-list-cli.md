---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 01/28/2020
ms.date: 05/25/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 3d31ec3d8a7275c3d2f984439cbfda0cc40203e1
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801276"
---
<!--Verify Successfully-->
## <a name="list-information"></a>列出信息

使用 [az sig list](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-list) 获取有关可用映像库的位置、状态和其他信息。

```azurecli 
az sig list -o table
```

使用 [az sig image-definition list](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-list) 列出库中的映像定义，包括有关 OS 类型和状态的信息。

```azurecli 
az sig image-definition list --resource-group myGalleryRG --gallery-name myGallery -o table
```

使用 [az sig image-version list](https://docs.microsoft.com/cli/azure/sig/image-version?view=azure-cli-latest#az-sig-image-version-list) 列出库中的共享映像版本。

```azurecli
az sig image-version list --resource-group myGalleryRG --gallery-name myGallery --gallery-image-definition myImageDefinition -o table
```

使用 [az sig image-version show](https://docs.microsoft.com/cli/azure/sig/image-version?view=azure-cli-latest#az-sig-image-version-show) 获取映像版本的 ID。

```azurecli
az sig image-version show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --query "id"
```

<!-- Update_Description: update meta properties, wording update, update link -->