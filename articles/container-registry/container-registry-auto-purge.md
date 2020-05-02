---
title: 清除标记和清单
description: 使用 purge 命令可以根据存留期和标记筛选器，从 Azure 容器注册表中删除多个标记和清单，并可以选择性地计划“清除”操作。
ms.topic: article
origin.date: 08/14/2019
ms.date: 04/06/2020
ms.author: v-yeche
ms.openlocfilehash: 64f949cb67568c096f5fe7945c834ce363f09d1d
ms.sourcegitcommit: 564739de7e63e19a172122856ebf1f2f7fb4bd2e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82093532"
---
# <a name="automatically-purge-images-from-an-azure-container-registry"></a>从 Azure 容器注册表中自动清除映像

使用 Azure 容器注册表作为开发工作流的一部分时，短期之后注册表内就会迅速填满不需要的映像或其他项目。 你可能想要删除存留期已超过特定持续时间或名称与指定的名称筛选器匹配的所有标记。 本文将会介绍 `acr purge` 命令，该命令可按需运行或作为[计划的](container-registry-tasks-scheduled.md) ACR 任务运行，以快速删除多个项目。 

`acr purge` 命令目前已在公共容器映像 (`mcr.microsoft.com/acr/acr-cli:0.1`) 中分发，该映像是基于 GitHub 上的 [acr-cli](https://github.com/Azure/acr-cli) 存储库中的源代码生成的。

可以使用本地安装的 Azure CLI 来运行本文中的 ACR 任务示例。 若要在本地使用 Azure CLI，需要安装 2.0.69 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI][azure-cli-install]。 

<!--Not Available on the Azure local Shell or-->

> [!IMPORTANT]
> 此功能目前以预览版提供。 需同意[补充使用条款][terms-of-use]才可使用预览版。 在正式版 (GA) 推出之前，此功能的某些方面可能会有所更改。

> [!WARNING]
> 请谨慎使用 `acr purge` 命令 -- 删除的映像数据不可恢复。 如果系统按清单摘要（而不是映像名称）拉取映像，那么你不应清除无标记的映像。 删除无标的记映像后，这些系统即无法从注册表拉取映像。 不按清单拉取，而是考虑采用[建议的最佳做法](container-registry-image-tag-version.md)，即“唯一标记”方案  。

若要使用 Azure CLI 命令删除单个映像标记或清单，请参阅[删除 Azure 容器注册表中的容器映像](container-registry-delete.md)。

## <a name="use-the-purge-command"></a>使用 purge 命令

`acr purge` 容器命令按标记删除存储库中的映像，这些映像的名称与一个名称筛选器匹配，并且这些映像的存留期超过指定的持续时间。 默认情况下，只会删除标记引用，而不删除基础[清单](container-registry-concepts.md#manifest)和层数据。 该命令还有一个可用于删除清单的选项。 

> [!NOTE]
> `acr purge` 不会删除 `write-enabled` 特性设置为 `false` 的映像标记或存储库。 有关详细信息，请参阅[锁定 Azure 容器注册表中的容器映像](container-registry-image-lock.md)。

`acr purge` 旨在作为 [ACR 任务](container-registry-tasks-overview.md)中的容器命令运行，以便向运行任务的注册表自动进行身份验证，并在其中执行操作。 本文中的任务示例使用 `acr purge` 命令[别名](container-registry-tasks-reference-yaml.md#aliases)来取代完全限定的容器映像命令。

运行 `acr purge` 时，请至少指定以下参数：

* `--filter` - 一个存储库，以及一个用于筛选该存储库中的标记的正则表达式  。 示例：`--filter "hello-world:.*"` 匹配 `hello-world` 存储库中的所有标记，`--filter "hello-world:^1.*"` 匹配以 `1` 开头的标记。 传递多个 `--filter` 参数可清除多个存储库。
* `--ago` - Go 样式的[持续时间字符串](https://golang.org/pkg/time/)，用于指示一个持续时间，存留期超过该持续时间的映像会被删除。 该持续时间包含由一个或多个十进制数组成的序列，其中的每个数字带有一个单位后缀。 有效时间单位包括“d”（表示天）、“h”（表示小时）和“m”（表示分钟）。 例如，`--ago 2d3h6m` 会选择上次修改时间在 2 天 3 小时 6 分钟以前的所有已筛选映像，而 `--ago 1.5h` 会选择上次修改时间在 1.5 小时以前的映像。

`acr purge` 支持多个可选参数。 本文中的示例使用了以下两个可选参数：

* `--untagged` - 指定删除没有关联标记的清单（无标记清单）  。
* `--dry-run` - 指定不删除任何数据，但输出与在不使用此标志的情况下运行命令后的结果相同。 此参数可用于测试 purge 命令，以确保它不会在无意中删除你想要保留的数据。

若要了解其他参数，请运行 `acr purge --help`。 

`acr purge` 支持 ACR 任务命令的其他功能，包括 [run 变量](container-registry-tasks-reference-yaml.md#run-variables)以及[任务运行日志](container-registry-tasks-logs.md)（这些日志会进行流式传输，还会进行保存供以后检索）。

### <a name="run-in-an-on-demand-task"></a>在按需任务中运行

以下示例使用 [az acr run][az-acr-run] 命令按需运行 `acr purge` 命令。 此示例将删除 myregistry 的 `hello-world` 存储库中修改时间在 1 天之前的所有映像标记和清单  。 容器命令是使用环境变量传递的。 该任务无需源代码上下文即可运行。

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge --filter 'hello-world:.*' \
  --untagged --ago 1d"

az acr run \
  --cmd "$PURGE_CMD" \
  --registry myregistry \
  /dev/null
```

### <a name="run-in-a-scheduled-task"></a>在计划的任务中运行

以下示例使用 [az acr task create][az-acr-task-create] 命令创建[计划的每日 ACR 任务](container-registry-tasks-scheduled.md)。 该任务清除 `hello-world` 存储库中修改时间在 7 天之前的标记。 容器命令是使用环境变量传递的。 该任务无需源代码上下文即可运行。

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge --filter 'hello-world:.*' \
  --ago 7d"

az acr task create --name purgeTask \
  --cmd "$PURGE_CMD" \
  --schedule "0 0 * * *" \
  --registry myregistry \
  --context /dev/null
```

运行 [az acr task show][az-acr-task-show] 命令查看该计时器触发器是否已配置。

### <a name="purge-large-numbers-of-tags-and-manifests"></a>清除大量标记和清单

清除大量标记和清单可能需要几分钟或更长时间。 若要清除数千个标记和清单，运行命令所需的时间可能会超过默认超时时间。对于按需任务，默认超时为 600 秒；对于计划的任务，默认超时为 3600 秒。 如果超过超时时间，只会删除一部分标记和清单。 为确保完成大规模清除，请传递 `--timeout` 参数来增大超时值。 

例如，以下按需任务将超时时间设置为 3600 秒（1 小时）：

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge --filter 'hello-world:.*' \
  --ago 1d --untagged"

az acr run \
  --cmd "$PURGE_CMD" \
  --registry myregistry \
  --timeout 3600 \
  /dev/null
```

## <a name="example-scheduled-purge-of-multiple-repositories-in-a-registry"></a>示例：计划清除注册表中的多个存储库

此示例演练如何使用 `acr purge` 定期清理注册表中的多个存储库。 例如，你可能会有一个开发管道，它将映像推送到 `samples/devimage1` 和 `samples/devimage2` 存储库。 你会定期将开发映像导入到用于部署的生产存储库，因此你不再需要这些开发映像。 你会每周清除 `samples/devimage1` 和 `samples/devimage2` 存储库，以便为未来一周的工作做好准备。

### <a name="preview-the-purge"></a>预览清除

在删除数据之前，我们建议使用 `--dry-run` 参数运行按需清除任务。 使用此选项可以查看命令要清除的标记和清单，而不会删除任何数据。 

在以下示例中，每个存储库中的筛选器会选择所有标记。 `--ago 0d` 参数匹配与筛选器相匹配的存储库中所有存留期的映像。 请根据自己的情况按需修改选择条件。 `--untagged` 参数指示除了删除标记以外，还要删除清单。 容器命令通过环境变量传递给 [az acr run][az-acr-run] 命令。

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge \
  --filter 'samples/devimage1:.*' --filter 'samples/devimage2:.*' \
  --ago 0d --untagged --dry-run"

az acr run \
  --cmd "$PURGE_CMD" \
  --registry myregistry \
  /dev/null
```

检查命令输出，以查看与选择参数匹配的标记和清单。 由于该命令是在使用 `--dry-run` 的情况下运行的，因此不会删除任何数据。

示例输出：

```console
[...]
Deleting tags for repository: samples/devimage1
myregistry.azurecr.cn/samples/devimage1:232889b
myregistry.azurecr.cn/samples/devimage1:a21776a
Deleting manifests for repository: samples/devimage1
myregistry.azurecr.cn/samples/devimage1@sha256:81b6f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e788b
myregistry.azurecr.cn/samples/devimage1@sha256:3ded859790e68bd02791a972ab0bae727231dc8746f233a7949e40f8ea90c8b3
Deleting tags for repository: samples/devimage2
myregistry.azurecr.cn/samples/devimage2:5e788ba
myregistry.azurecr.cn/samples/devimage2:f336b7c
Deleting manifests for repository: samples/devimage2
myregistry.azurecr.cn/samples/devimage2@sha256:8d2527cde610e1715ad095cb12bc7ed169b60c495e5428eefdf336b7cb7c0371
myregistry.azurecr.cn/samples/devimage2@sha256:ca86b078f89607bc03ded859790e68bd02791a972ab0bae727231dc8746f233a

Number of deleted tags: 4
Number of deleted manifests: 4
[...]
```

### <a name="schedule-the-purge"></a>计划清除

验证试运行后，创建一个计划任务以自动执行清除。 以下示例计划一个在每周星期日 1:00（UTC 时间）运行上述 purge 命令的任务：

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge \
  --filter 'samples/devimage1:.*' --filter 'samples/devimage2:.*' \
  --ago 0d --untagged"

az acr task create --name weeklyPurgeTask \
  --cmd "$PURGE_CMD" \
  --schedule "0 1 * * Sun" \
  --registry myregistry \
  --context /dev/null
```

运行 [az acr task show][az-acr-task-show] 命令查看该计时器触发器是否已配置。

## <a name="next-steps"></a>后续步骤

了解用于[删除 Azure 容器注册表中的映像数据](container-registry-delete.md)的其他选项。

有关映像存储的详细信息，请参阅 [Azure 容器注册表中的容器映像存储](container-registry-storage.md)。

<!-- LINKS - External -->

[terms-of-use]: https://www.azure.cn/support/legal/subscription-agreement/

<!-- LINKS - Internal -->

[azure-cli-install]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest
[az-acr-run]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-run
[az-acr-task-create]: https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-create
[az-acr-task-show]: https://docs.azure.cn/cli/acr/task?view=azure-cli-latest#az-acr-task-show

<!-- Update_Description: new article about container registry auto purge -->
<!--NEW.date: 04/06/2020-->