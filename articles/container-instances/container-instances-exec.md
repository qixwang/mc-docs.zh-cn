---
title: 在运行的容器实例中执行命令
description: 了解如何在 Azure 容器实例上当前正在运行的容器中执行命令
ms.topic: article
origin.date: 03/30/2018
ms.date: 01/15/2020
ms.author: v-yeche
ms.openlocfilehash: 4df00eddd23f104a17d513fea11b895dcbfc0cd9
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428742"
---
<!--Verified successfully-->
# <a name="execute-a-command-in-a-running-azure-container-instance"></a>在运行的 Azure 容器实例中执行命令

Azure 容器实例支持在运行的容器中执行命令。 在应用程序开发和故障排除期间，在已启动的容器中运行命令特别有用。 此功能的最常见用途是启动交互式 shell，以便能够在运行的容器中调试问题。

## <a name="run-a-command-with-azure-cli"></a>使用 Azure CLI 运行命令

在 [Azure CLI][azure-cli] 中使用 [az container exec][az-container-exec] 在运行的容器中执行命令：

```azurecli
az container exec --resource-group <group-name> --name <container-group-name> --exec-command "<command>"
```

例如，若要在 Nginx 容器中启动 Bash shell：

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
```

在以下示例输出中，Bash shell 已在运行的 Linux 容器中启动，提供一个可用于执行 `ls` 的终端：

```console
$ az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# exit
exit
Bye.
```

<!--Not Available on WINDOWS CONTAINER-->



## <a name="multi-container-groups"></a>多容器组

如果[容器组](container-instances-container-groups.md)包含多个容器（例如应用程序容器和日志记录分支），请使用 `--container-name` 指定要在其中运行命令的容器的名称。

例如，容器组 *mynginx* 包含两个容器：*nginx-app* 和 *logger*。 若要在 *nginx-app* 容器中启动 shell：

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --container-name nginx-app --exec-command "/bin/bash"
```

## <a name="restrictions"></a>限制

Azure 容器实例目前支持使用 [az container exec][az-container-exec] 启动单个进程；不能传递命令参数。 例如，不能像 `sh -c "echo FOO && echo BAR"` 一样串接命令，或执行 `echo FOO`。

## <a name="next-steps"></a>后续步骤

在[排查 Azure 容器实例中的容器和部署问题](container-instances-troubleshooting.md)中了解其他故障排除工具和常见部署问题。

<!-- LINKS - internal -->

[az-container-create]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-exec]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-exec
[azure-cli]: https://docs.azure.cn/cli/index?view=azure-cli-latest

<!-- Update_Description: new article about container instances exec -->
<!--NEW.date: 01/15/2020-->