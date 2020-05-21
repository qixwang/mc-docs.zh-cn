---
title: include 文件
description: include 文件
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 07/12/2019
ms.date: 05/18/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 9470af4b40da5f833e93fd64610c53323e7dc718
ms.sourcegitcommit: 8d56bc6baeb42d675695ecef1909d76f5c4a6ae3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83406206"
---
### <a name="create-a-user-assigned-identity"></a>创建用户分配的标识

使用 [az identity create][az-identity-create] 命令在订阅中创建名为 myACRTasksId 的标识。 可以使用先前用于创建容器注册表的相同资源组，也可以使用不同的资源组。

```azurecli
az identity create --resource-group myResourceGroup --name myACRTasksId
```

若要在以下步骤中配置用户分配的标识，请使用 [az identity show][az-identity-show] 命令将标识的资源 ID、主体 ID 和客户端 ID 存储在变量中。

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get principal ID of the task's user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query clientId --output tsv)
```

<!-- LINKS - Internal -->

[az-identity-create]: https://docs.microsoft.com/cli/azure/identity?view=azure-cli-latest#az-identity-create
[az-identity-show]: https://docs.microsoft.com/cli/azure/identity?view=azure-cli-latest#az-identity-show

<!-- Update_Description: update meta properties, wording update, update link -->