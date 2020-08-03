---
title: include 文件
description: include 文件
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 07/12/2019
ms.date: 08/03/2020
ms.testscope: no
ms.testdate: 05/18/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 4ae38c0f4f31741e2cdd18f512a3a58523ee9a7a
ms.sourcegitcommit: 692b9bad6d8e4d3a8e81c73c49c8cf921e1955e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2020
ms.locfileid: "87427556"
---
### <a name="create-a-user-assigned-identity"></a>创建用户分配的标识

使用 [az identity create][az-identity-create] 命令在订阅中创建一个名为 *myACRTasksId* 的标识。 可以使用之前用于创建容器注册表的同一资源组，也可以使用其他资源组。

```azurecli
az identity create \
  --resource-group myResourceGroup \
  --name myACRTasksId
```

为了在以下步骤中配置用户分配的标识，请使用 [az identity show][az-identity-show] 命令将标识的资源 ID、主体 ID 和客户端 ID 存储在变量中。

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query id --output tsv)

# Get principal ID of the task's user-assigned identity
principalID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query clientId --output tsv)
```

<!-- LINKS - Internal -->

[az-identity-create]: https://docs.microsoft.com/cli/azure/identity?view=azure-cli-latest#az-identity-create
[az-identity-show]: https://docs.microsoft.com/cli/azure/identity?view=azure-cli-latest#az-identity-show

<!-- Update_Description: update meta properties, wording update, update link -->