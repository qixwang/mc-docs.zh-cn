---
title: include 文件
description: include 文件
services: app-service
author: msangapu-msft
ms.service: app-service
ms.topic: include
origin.date: 02/02/2018
ms.date: 03/23/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 57c79d9217fe54ea559f5943d4975bbdac578f76
ms.sourcegitcommit: d5eca3c6b03b206e441b599e5b138bd687a91361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2020
ms.locfileid: "78934716"
---
在 `myAppServicePlan` 应用服务计划中创建一个 Web 应用。 

在 Azure CLI 中，可以使用 [`az webapp create`](/cli/webapp?view=azure-cli-latest#az-webapp-create) 命令。在以下示例中，将 `<app-name>` 替换为全局唯一的应用名称（有效字符是 `a-z`、`0-9` 和 `-`）。 运行时设置为 `PHP|7.3`。 若要查看所有受支持的运行时，请运行 [az webapp list-runtimes](/cli/webapp?view=azure-cli-latest#az-webapp-list-runtimes)。 

```azurecli
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "PHP|7.3" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "PHP|7.3" --deployment-local-git
```

创建 Web 应用后，Azure CLI 会显示类似于以下示例的输出：

```json
Local git is configured with url of 'https://<username>@<app-name>.scm.chinacloudsites.cn/<app-name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.chinacloudsites.cn",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.chinacloudsites.cn/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

现在你已经创建了一个新的空 Web 应用并启用了 Git 部署。

> [!NOTE]
> Git 远程的 URL 将显示在 `deploymentLocalGitUrl` 属性中，其格式为 `https://<username>@<app-name>.scm.chinacloudsites.cn/<app-name>.git`。 保存此 URL，后续将会用到。
>
