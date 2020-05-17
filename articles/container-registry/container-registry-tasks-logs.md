---
title: 查看任务运行日志 - 任务
description: 如何查看和管理 ACR 任务生成的运行日志。
ms.topic: article
origin.date: 03/09/2020
ms.date: 05/08/2020
ms.author: v-yeche
ms.openlocfilehash: 7c1ca6a9b88d812fe7fbd8357f7785b830ce2a9b
ms.sourcegitcommit: 2d8950c6c255361eb6c66406988e25c69cf4e0f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83392437"
---
<!--Verified successfully-->
# <a name="view-and-manage-task-run-logs"></a>查看和管理任务运行日志

[Azure 容器注册表任务](container-registry-tasks-overview.md)中的每个任务运行都会生成日志输出，检查该输出即可确定任务步骤是否已成功运行。 

本文介绍了如何查看和管理任务运行日志。

## <a name="view-streamed-logs"></a>查看流式传输的日志

手动触发任务时，日志输出会直接流式传输到控制台。 例如，使用 [az acr build](https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-build)、[az acr run](https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-run) 或 [az acr task run](https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-run) 命令手动触发任务时，你会看到流式传输到控制台的日志输出。 

以下示例 [az acr run](https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-run) 命令手动触发一个任务，该任务运行从同一注册表中拉取的容器：

```azurecli
az acr run --registry mycontainerregistry1220 \
  --cmd '$Registry/samples/hello-world:v1' /dev/null
```

流式传输的日志：

```console
Queued a run with ID: cf4
Waiting for an agent...
2020/03/09 20:30:10 Alias support enabled for version >= 1.1.0, please see https://aka.ms/acr/tasks/task-aliases for more information.
2020/03/09 20:30:10 Creating Docker network: acb_default_network, driver: 'bridge'
2020/03/09 20:30:10 Successfully set up Docker network: acb_default_network
2020/03/09 20:30:10 Setting up Docker configuration...
2020/03/09 20:30:11 Successfully set up Docker configuration
2020/03/09 20:30:11 Logging in to registry: mycontainerregistry1220.azurecr.cn
2020/03/09 20:30:12 Successfully logged into mycontainerregistry1220.azurecr.cn
2020/03/09 20:30:12 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2020/03/09 20:30:12 Launching container with name: acb_step_0
Unable to find image 'mycontainerregistry1220.azurecr.cn/samples/hello-world:v1' locally
v1: Pulling from samples/hello-world
Digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e888a
Status: Downloaded newer image for mycontainerregistry1220.azurecr.cn/samples/hello-world:v1

Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]

2020/03/09 20:30:13 Successfully executed container: acb_step_0
2020/03/09 20:30:13 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.180081)

Run ID: cf4 was successful after 5s
```

## <a name="view-stored-logs"></a>查看存储的日志 

Azure 容器注册表存储所有任务的运行日志。 可以在 Azure 门户中查看已存储的运行日志。 也可使用 [az acr task logs](https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-logs) 命令查看选定的日志。 默认情况下，日志保留 30 天。

如果任务是自动触发的（例如，由源代码更新触发），则访问已存储的日志是查看运行日志的唯一方法。  任务自动触发器包括源代码提交或拉取请求、基础映像更新和计时器触发器。

若要在门户中查看运行日志，请执行以下操作：

1. 导航到你的容器注册表。
1. 在“服务”  中，选择  “任务” > “运行”  。
1. 选择一个“运行 ID”  来查看运行状态和运行日志。 运行日志包含与流式传输的日志（如果生成了该日志）相同的信息。

![查看任务运行登录门户](./media/container-registry-tasks-logs/portal-task-run-logs.png)

若要使用 Azure CLI 查看日志，请运行 [az acr task logs](https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-logs) 并指定运行 ID、任务名称，或指定由生成任务创建的特定映像。 如果指定了任务名称，则该命令会显示上次创建的运行的日志。

以下示例输出 ID 为 cf4  的运行的日志：

```azurecli
az acr task logs --registry mycontainerregistry1220 \
  --run-id cf4
```

## <a name="alternative-log-storage"></a>替代日志存储

你可能希望将任务运行日志存储在本地文件系统上，或使用替代存档解决方案，例如 Azure 存储。

例如，创建一个本地 tasklogs  目录，并将 [az acr task logs](https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-logs) 的输出重定向到一个本地文件：

```azurecli
mkdir ~/tasklogs

az acr task logs --registry mycontainerregistry1220 \
  --run-id cf4 > ~/tasklogs/cf4.log
```

还可以将本地日志文件保存到 Azure 存储。 例如，使用 [Azure CLI](../storage/blobs/storage-quickstart-blobs-cli.md)、[Azure 门户](../storage/blobs/storage-quickstart-blobs-portal.md)或其他方法将文件上传到某个存储帐户。

## <a name="next-steps"></a>后续步骤

* 详细了解 [Azure 容器注册表任务](container-registry-tasks-overview.md)

<!-- LINKS - External -->

[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://www.azure.cn/support/legal/subscription-agreement/

<!-- LINKS - Internal -->

[azure-cli]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest
[az-acr-build]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-build
[az-acr-pack-build]: https://docs.microsoft.com/cli/azure/acr/pack?view=azure-cli-latest#az-acr-pack-build
[az-acr-task]: https://docs.azure.cn/cli/acr/task?view=azure-cli-latest
[az-acr-task-create]: https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-create
[az-acr-task-run]: https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-run
[az-acr-task-update]: https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-update
[az-login]: https://docs.azure.cn/cli/reference-index?view=azure-cli-latest#az-login
[az-login-service-principal]: https://docs.azure.cn/cli/authenticate-azure-cli?view=azure-cli-latest

<!-- IMAGES -->

[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png

<!-- Update_Description: new article about container registry tasks logs -->
<!--NEW.date: 04/06/2020-->