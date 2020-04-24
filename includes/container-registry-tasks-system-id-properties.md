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
ms.openlocfilehash: 830e2981d74ad8a8283fd5702db0330a7369c5a3
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70134523"
---
在命令输出中，`identity` 部分显示在任务中设置了 `SystemAssigned` 类型的标识。 `principalId` 是标识的服务主体 ID：

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "chinanorth",
[...]
``` 
使用 [az acr task show][az-acr-task-show] 命令将 principalId 存储在变量中，以便在后面的命令中使用：

```azurecli
principalID=$(az acr task show --name dockerhubtask --registry myregistry --query identity.principalId --output tsv)
```

<!-- LINKS - Internal -->

[az-acr-task-show]: https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-show
