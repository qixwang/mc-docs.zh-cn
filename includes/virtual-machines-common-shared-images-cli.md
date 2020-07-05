---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/24/2020
ms.date: 07/06/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 667df6794286db3961afa695baf7d8990f0496ff
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945764"
---
## <a name="create-an-image-gallery"></a>创建映像库 

映像库是用于启用映像共享的主要资源。 

允许用于库名称的字符为大写或小写字母、数字、点和句点。 库名称不能包含短划线。   库名称在你的订阅中必须唯一。 

使用 [az sig create](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-create) 创建一个映像库。 以下示例在“中国东部”创建名为 myGalleryRG 的资源组，以及名为 myGallery 的库  。

```azurecli
az group create --name myGalleryRG --location chinaeast
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="share-the-gallery"></a>共享库

可以使用基于角色的访问控制 (RBAC) 在订阅之间共享映像。 可以在库、映像定义或映像版本级别共享映像。 任何对映像版本具有读取权限的用户，即使跨订阅，也能够使用映像版本部署 VM。

建议在库级别与其他用户进行共享。 若要获取库的对象 ID，请使用 [az sig show](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-show)。

```azurecli
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

使用对象 ID 作为范围，并使用电子邮件地址和 [az role assignment create](https://docs.azure.cn/cli/role/assignment?view=azure-cli-latest#az-role-assignment-create) 为用户授予对共享映像库的访问权限。 请将 `<email-address>` 和 `<gallery iD>` 替换为自己的信息。

```azurecli
az role assignment create \
   --role "Reader" \
   --assignee <email address> \
   --scope <gallery ID>
```

有关如何使用 RBAC 共享资源的详细信息，请参阅[使用 RBAC 和 Azure CLI 管理访问权限](/role-based-access-control/role-assignments-cli)。

<!-- Update_Description: update meta properties, wording update, update link -->