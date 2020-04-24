---
title: include 文件
description: include 文件
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 07/12/2019
ms.date: 08/26/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 1140092cd371da914cdefec59aae03d2c64de272
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70134524"
---
### <a name="create-a-user-assigned-identity"></a>创建用户分配的标识

使用 [az identity create][az-identity-create] 命令在订阅中创建名为 myACRTasksId  的标识。 可以使用先前用于创建容器注册表的相同资源组，也可以使用不同的资源组。

```azurecli
az identity create --resource-group myResourceGroup --name myACRTasksId
```

若要在以下步骤中配置用户分配的标识，请使用 [az identity show][az-identity-show] 命令将标识的资源 ID、服务主体 ID 和客户端 ID 存储在变量中。

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query clientId --output tsv)
```

<!-- LINKS - Internal -->

[az-identity-create]: https://docs.azure.cn/cli/identity?view=azure-cli-latest#az-identity-create
[az-identity-show]: https://docs.azure.cn/cli/identity?view=azure-cli-latest#az-identity-show
