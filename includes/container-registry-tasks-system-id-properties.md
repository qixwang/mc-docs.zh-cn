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
ms.openlocfilehash: 2a234b04dd8ae7ae4a237ecaecaf021a2d390d23
ms.sourcegitcommit: 8d56bc6baeb42d675695ecef1909d76f5c4a6ae3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83406223"
---
在命令输出中，`identity` 部分显示在任务中设置了 `SystemAssigned` 类型的标识。 `principalId` 是任务标识的主体 ID：

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
使用 [az acr task show][az-acr-task-show] 命令将 principalId 存储在变量中，以便在后面的命令中使用。 在以下命令中替换任务名称和注册表：

```azurecli
principalID=$(az acr task show --name mytask --registry myregistry --query identity.principalId --output tsv)
```

<!-- LINKS - Internal -->

[az-acr-task-show]: https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-show

<!-- Update_Description: update meta properties, wording update, update link -->