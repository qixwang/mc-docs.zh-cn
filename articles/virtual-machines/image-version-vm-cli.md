---
title: 从 VM 创建映像
description: 了解如何在共享映像库中从 Azure 中的 VM 创建映像。
author: rockboyfor
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
origin.date: 05/01/2020
ms.date: 06/01/2020
ms.author: v-yeche
ms.reviewer: akjosh
ms.openlocfilehash: 9134c639644f85043f86b3a72a5be03b0570bb83
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801816"
---
<!--Verified successfully-->
# <a name="create-an-image-version-from-a-vm-in-azure-using-the-azure-cli"></a>使用 Azure CLI 从 Azure 中的 VM 创建映像版本

如果要使用现有 VM 生成多个相同的 VM，可以通过 Azure CLI 使用该 VM 在共享映像库中创建映像。 还可以使用 [Azure PowerShell](image-version-vm-powershell.md) 从 VM 创建映像。

使用共享映像库时，将使用**映像版本**来创建 VM。 可根据环境的需要创建多个映像版本。 在使用某个映像版本创建 VM 时，将使用该映像版本为新 VM 创建磁盘。 可以多次使用映像版本。

## <a name="before-you-begin"></a>准备阶段

若要完成本文，必须具有现有的共享映像库。 

在 Azure 中，在你的库所在区域中还必须有一个现有 VM。 

如果 VM 附加了数据磁盘，则数据磁盘大小不能超过 1 TB。

通过本文进行操作时，请根据需要替换资源名称。

## <a name="get-information-about-the-vm"></a>获取有关 VM 的信息

可以使用 [az vm list](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-list) 查看可用 VM 的列表。 

```azurecli
az vm list --output table
```

知道 VM 的名称及其所在的资源组后，使用 [az vm get-instance-view](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-get-instance-view) 获取 VM 的 ID。 

```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVm --query id
```

## <a name="create-an-image-definition"></a>创建映像定义

映像定义为映像创建一个逻辑分组。 它们用于管理有关映像版本的信息，这些版本是在其中创建的。 

映像定义名称可以由大写或小写字母、数字、点、短划线和句点构成。 

请确保映像定义的类型正确。 如果已通用化 VM（使用适用于 Windows 的 Sysprep，或适用于 Linux 的 waagent -deprovision），则应使用 `--os-state generalized` 创建通用化映像定义。 若要在不删除现有用户帐户的情况下使用 VM，请使用 `--os-state specialized` 创建专用化映像定义。

若要详细了解可为映像定义指定的值，请参阅[映像定义](/virtual-machines/linux/shared-image-galleries#image-definitions)。

使用 [az sig image-definition create](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-create) 在库中创建一个映像定义。

在此示例中，映像定义名为 myImageDefinition，适用于[专用化](/virtual-machines/linux/shared-image-galleries#generalized-and-specialized-images) Linux OS 映像。 若要使用 Windows OS 创建映像的定义，请使用 `--os-type Windows`。 

```azurecli 
az sig image-definition create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku mySKU \
   --os-type Linux \
   --os-state specialized
```

## <a name="create-the-image-version"></a>创建映像版本

使用 [az image gallery create-image-version](https://docs.microsoft.com/cli/azure/sig/image-version?view=azure-cli-latest#az-sig-image-version-create) 从 VM 创建映像版本。  

允许用于映像版本的字符为数字和句点。 数字必须在 32 位整数范围内。 格式：*MajorVersion*.*MinorVersion*.*Patch*。

在此示例中，映像的版本为 *1.0.0*，并且我们打算使用本地冗余存储在“中国北部”区域创建 2 个副本，在“中国东部区域”创建 1 个副本，在“中国东部 2”区域创建 1 个副本。 复制区域必须包含源 VM 所在的区域。

<!--MOONCAKE: CORRECT ON locally-redundant storage-->

请将此示例中的 `--managed-image` 值替换为上一步的 VM ID。

```azurecli 
az sig image-version create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "chinanorth" "chinaeast=1" "chinaeast2=1=standard_lrs" \
   --replica-count 2 \
   --managed-image "/subscriptions/<Subscription ID>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM"
```

<!--CORRECT ON --target-regions "chinanorth" "chinaeast=1" "chinaeast2=1=standard_lrs"

> [!NOTE]
> You need to wait for the image version to completely finish being built and replicated before you can use the same managed image to create another image version.
>
> You can also store your image in Premiun storage by a adding `--storage-account-type  premium_lrs`, or locally Redundant Storage by adding `--storage-account-type  standard_lrs` when you create the image version.
>

<!--CORRECT ON or locally Redundant Storage by adding `--storage-account-type  standard_lrs`-->

## <a name="next-steps"></a>后续步骤

使用 Azure CLI 从[通用化映像](vm-generalized-image-version-cli.md)创建 VM。

<!-- Update_Description: new article about image version vm cli -->
<!--NEW.date: 06/01/2020-->