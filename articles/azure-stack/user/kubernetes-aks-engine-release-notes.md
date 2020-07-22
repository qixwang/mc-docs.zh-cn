---
title: Azure Stack Hub 上的 Azure Kubernetes 服务 (AKS) 引擎发行说明
description: 了解更新 Azure Stack Hub 上的 AKS 引擎需要采取的步骤。
author: WenJason
ms.topic: article
origin.date: 06/29/2020
ms.date: 07/20/2020
ms.author: v-jay
ms.reviewer: waltero
ms.lastreviewed: 06/25/2020
ms.openlocfilehash: 4528d267d615a20ed1c220d5e9b6c6ff4be0740c
ms.sourcegitcommit: e9ffd50aa5eaab402a94bfabfc70de6967fe6278
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86307390"
---
# <a name="release-notes-for-the-aks-engine-on-azure-stack-hub"></a>Azure Stack Hub 上的 AKS 引擎发行说明
::: moniker range=">=azs-2002"
适用于 ASK 引擎的版本 0.51.0。

本文介绍了 Azure Stack Hub 上的 Azure Kubernetes 服务 (AKS) 引擎更新的内容。 此更新包括对面向 Azure Stack Hub 平台的最新版 AKS 引擎的改进和修复。 请注意，本文并不是为了记录 Azure 的 AKS 引擎的发布信息。

## <a name="update-planning"></a>更新规划

AKS 引擎升级命令完全自动执行群集的升级过程，它负责处理虚拟机 (VM)、网络、存储、Kubernetes 和业务流程任务。 应用更新之前，请务必查看本文中的信息。

### <a name="upgrade-considerations"></a>升级注意事项

-   你是否使用了正确的市场项（即 AKS 基础 Ubuntu 16.04-LTS 映像发行版）作为你的 AKS 引擎版本？ 可以在“下载新映像和 AKS 引擎”部分中找到版本。

-   是否为目标群集使用了正确的群集规范 (`apimodel.json`) 和资源组？ 最初部署群集时，已在输出目录中生成此文件。 请参阅[部署 Kubernetes 群集](/azure-stack/user/azure-stack-kubernetes-aks-engine-deploy-cluster#deploy-a-kubernetes-cluster)中的 `deploy` 命令参数。

-   是否使用了可靠的计算机来运行 AKS 引擎并从中执行升级操作？

-   如果要更新具有活动工作负荷的正常运转中的群集，假如群集处于正常负载下，则你可以在不影响它们的情况下应用升级。 但是，你应该有一个备份群集，以防需要将用户重定向到它。 强烈建议使用备份群集。

-   如果可能，请从 Azure Stack Hub 环境中的 VM 运行命令，以减少网络跃点和潜在的连接故障。

-   请确保订阅有足够的配额来完成整个过程。 进程将在此过程中分配新的 VM。 生成的 VM 的数量将与原始数量相同，但要对在该过程中创建更多的 VM 做好计划。

-   未规划系统更新或计划的任务。

-   在已使用与生产群集相同的值配置的群集上设置分阶段升级，并在其上测试升级，然后再在生产群集中执行此操作。

### <a name="use-the-upgrade-command"></a>使用升级命令

你需要使用 AKS 引擎的 `upgrade` 命令，如下面的文章[升级 Azure Stack Hub 上的 Kubernetes 群集](/azure-stack/user/azure-stack-kubernetes-aks-engine-upgrade)所述。

### <a name="upgrade-interruptions"></a>升级中断

有时，意外的因素会中断群集的升级。 当 AKS 引擎报告了某个错误或在 AKS 引擎执行过程中发生某些事情时，可能会发生中断。 请检查中断的原因，解决问题，然后再次提交相同的升级命令以继续执行升级过程。 **upgrade** 命令是幂等的，在重新提交命令后应该会恢复群集的升级。 通常，中断会增加完成更新所需的时间，但不会影响它的完成。

### <a name="estimated-upgrade-time"></a>估计的升级时间

群集中每个 VM 的估计时间为 12 到 15 分钟。 例如，一个 20 节点的群集可能需要大约五 (5) 小时才能完成升级。

## <a name="download-new-image-and-aks-engine"></a>下载新映像和 AKS 引擎

下载 AKS 基础 Ubuntu 映像和 AKS 引擎的新版本。

如 Azure Stack Hub 的 AKS 引擎文档中所述，部署 Kubernetes 群集需要两个主要组件：

-   aks-engine 二进制文件

-   AKS 基础 Ubuntu 16.04-LTS 映像发行版

此更新提供了这些组件的新版本：

-   Azure Stack Hub 操作员需要将新的 AKS 基础 Ubuntu 映像下载到缩放单元市场：

    -   名称：AKS 基础 Ubuntu 16.04-LTS 映像发行版 2020 年 5 月 (2020.05.13)

    -   版本：2020.05.13

    -   按照[将 Azure Kubernetes 服务 (AKS) 引擎必备组件添加到 Azure Stack Hub 市场](/azure-stack/operator/azure-stack-aks-engine)一文中的说明进行操作

-   Kubernetes 群集管理员将需要下载新的 aks-engine 版本 0.51.0。 请参阅下面的文章[在 Linux 上的 Azure Stack Hub 中安装 AKS 引擎](/azure-stack/user/azure-stack-kubernetes-aks-engine-deploy-linux)中的说明。 你可以遵循用于首次安装群集的过程。 此更新将覆盖以前的二进制文件。 例如，如果你使用了 get-akse.sh 脚本，请按照[在联网环境中安装](/azure-stack/user/azure-stack-kubernetes-aks-engine-deploy-linux#install-in-a-connected-environment)部分中所述的步骤进行操作。 如果你是在 Windows 系统上安装，则此过程同样适用，请参阅[在 Windows 上的 Azure Stack Hub 中安装 AKS 引擎](/azure-stack/user/azure-stack-kubernetes-aks-engine-deploy-windows)一文。

## <a name="aks-engine-and-azure-stack-version-mapping"></a>AKS 引擎和 Azure Stack 版本映射

| Azure Stack Hub 版本 | AKS 引擎版本 |
| ----------------------------- | ------------------------ |
| 1910 | 0.43.0、0.43.1 |
| 2002 | 0.48.0、0.51.0 |

## <a name="kubernetes-version-upgrade-path-in-aks-engine-0510"></a>AKS 引擎 0.51.0 中的 Kubernetes 版本升级路径

可以在下表中找到 Azure Stack Hub 的当前版本和升级版本。 不要遵循 aks-engine get-versions 命令，因为该命令还包括全球 Azure 中支持的版本。 以下版本和升级表适用于 Azure Stack Hub 中的 AKS 引擎群集。

| 当前版本 | 可用升级 |
| ------------------------- | ----------------------- |
| 1.14.7、1.14.8 | 1.15.12 |
| 1.15.4、1.15.5、1.15.10 | 1.15.12、1.16.9 |
| 1.15.12 | 1.16.9 |
| 1.16.8 | 1.16.9 |

在 API 模型 json 文件中，请在 `orchestratorProfile` 部分下指定版本和版本值，例如，如果计划部署 Kubernetes 1.16.9，则必须设置以下两个值（请参阅示例 [kubernetes-azurestack.json](https://raw.githubusercontent.com/Azure/aks-engine/master/examples/azure-stack/kubernetes-azurestack.json)）：

```json  
    -   "orchestratorRelease": "1.16",
    -   "orchestratorVersion": "1.16.9"
```

## <a name="whats-new"></a>新增功能

-   添加了对 Kubernetes 1.15.12 的支持 ([#3212](https://github.com/Azure/aks-engine/issues/3212))
-   添加了对 Kubernetes 1.16.8 和 1.16.9 的支持（[#3157](https://github.com/Azure/aks-engine/issues/3157) 和 [#3087](https://github.com/Azure/aks-engine/issues/3087)）
-   Kubernetes 仪表板加载项 v2.0.0 ([#3140](https://github.com/Azure/aks-engine/issues/3140))
-   在 apt 操作已完成后删除多余的“ wait for no held apt locks”，以提高操作可靠性 ([#3049](https://github.com/Azure/aks-engine/issues/3049))
-   即使在连接到 K8s API 服务器时收到错误也完成脚本运行，以提高操作可靠性 ([#3022](https://github.com/Azure/aks-engine/issues/3022))
-   在连接前检查 DNS 配置状态，以提高操作可靠性 ([#3002](https://github.com/Azure/aks-engine/issues/3002))
-   修复了在为 windows 节点预配 kubenet 时出现的问题 ([#3300](https://github.com/Azure/aks-engine/issues/3300))
-   进行了修复以启动 systemd 和 etcd，从而确保幂等进程 ([#3126](https://github.com/Azure/aks-engine/issues/3126))
-   删除了对 Kubernetes 1.13 和 1.14 的支持（[#3310](https://github.com/Azure/aks-engine/issues/3310) 和 [#3059](https://github.com/Azure/aks-engine/issues/3059)）
-   确保了 pod-security-policy 是加载的第一个加载项 ([#3313](https://github.com/Azure/aks-engine/issues/3313))
-   已将 Azure CNI 版本更新为 v 1.1.0 ([#3075](https://github.com/Azure/aks-engine/issues/3075))（预览版）
-   添加了功能和修补程序以支持 Azure Stack Hub（预览版）中的 Windows 容器：
    -   对 Windows 版本收集进行了修复 ([#2954](https://github.com/Azure/aks-engine/issues/2954))
    -   更新了 Azure Stack Windows 二进制组件名称 ([#3231](https://github.com/Azure/aks-engine/issues/3231))
    -   更新了 Azure Stack Hub 上的 Windows 映像验证 ([#3260](https://github.com/Azure/aks-engine/issues/3260))
    -   更新了 Windows VHD 以包括五月修补程序 ([#3263](https://github.com/Azure/aks-engine/issues/3263))
    -   使用 4B 修补程序更新了 Windows VHD ([#3115](https://github.com/Azure/aks-engine/issues/3115))
    -   更新了 Windows VHD 以包括四月修补程序 ([#3101](https://github.com/Azure/aks-engine/issues/3101))

## <a name="known-issues"></a>已知问题

-   在单个群集内并行部署多个 Kubernetes 服务可能会导致基本负载均衡器配置错误。 如果可能，请一次部署一个服务。
-   此版本不支持 Kubernetes 1.17。 尽管有 PR 暗示 Kubernetes 1.17 受支持，但 1.17 实际上不受支持。
-   运行 aks-engine get-versions 将生成适用于 Azure 和 Azure Stack Hub 的信息，但是，没有明确的方法来识别与 Azure Stack Hub 对应的内容。 不要使用此命令来确定可升级的版本区域。 请使用上述升级参考表。
-   因为 aks-engine 工具是一个跨 Azure 和 Azure Stack Hub 的共享源代码存储库， 查看许多发行说明和拉取请求会使你相信该工具支持上面所列版本之外的其他版本的 Kubernetes 和 OS 平台，请忽略它们并使用上面的版本表作为此更新的官方指南。
- 在将 Kubernetes 群集从版本 1.15.x 升级 (aks-engine upgrade) 到 1.16.x 的过程中，不会升级 Kubernetes kube-proxy 组件：
  - 在联网环境中，此问题可能并不明显，因为群集中没有任何迹象表明 kube-proxy 未升级。 一切似乎都按预期进行。
  - 在离线环境中，当你运行以下查询来了解系统 pod 的状态时，你会发现此问题，并会发现 **kube-proxy** pod 未处于 **Ready**状态：`kubectl get pods -n kube-system`。
  - 若要解决此问题，请删除 kube-proxy deamonset，以便 Kubernetes 使用正确的版本重新启动它。 运行以下命令：`kubectl delete ds kube-proxy -n kube-system`。

## <a name="reference"></a>参考

这是组合了 Azure 和 Azure Stack Hub 的发行说明的完整集合：

-   https://github.com/Azure/aks-engine/releases/tag/v0.49.0
-   https://github.com/Azure/aks-engine/releases/tag/v0.50.0
-   https://github.com/Azure/aks-engine/releases/tag/v0.51.0
::: moniker-end
::: moniker range="<=azs-1910"
适用于 ASK 引擎的 0.48.0 版或更低版本。

本文介绍了 Azure Stack Hub 上的 Azure Kubernetes 服务 (AKS) 引擎更新的内容。 此更新包括对面向 Azure Stack Hub 平台的最新版 AKS 引擎的改进和修复。 请注意，本文的用途并非记录适用于全球 Azure 的 AKS 引擎的发布信息。

## <a name="update-planning"></a>更新规划

AKS 引擎升级命令完全自动执行群集的升级过程，它负责处理虚拟机 (VM)、网络、存储、Kubernetes 和业务流程任务。 应用更新之前，请务必查看本文中的信息。

### <a name="upgrade-considerations"></a>升级注意事项

-   你是否使用了正确的市场项（即 AKS 基础 Ubuntu 16.04-LTS 映像发行版）作为你的 AKS 引擎版本？ 可以在[下载新的 AKS 基础 Ubuntu 映像和 AKS 引擎版本](#download-new-image-and-aks-engine)部分中找到版本。

-   是否为目标群集使用了正确的群集规范 (apimodel.json) 和资源组？ 最初部署群集时，已在输出目录中生成此文件。 请参阅[部署 Kubernetes 群集](/azure-stack/user/azure-stack-kubernetes-aks-engine-deploy-cluster#deploy-a-kubernetes-cluster)中的“deploy”命令参数。

-   是否使用了可靠的计算机来运行 AKS 引擎并从中执行升级操作？

-   如果要更新具有活动工作负荷的正常运转中的群集，假如群集处于正常负载下，则你可以在不影响它们的情况下应用升级。 但是，你应该有一个备份群集，以防需要将用户重定向到它。

-   如果可能，请从 Azure Stack Hub 环境中的 VM 运行命令，以减少网络跃点和潜在的连接故障。

-   请确保订阅有足够的配额来完成整个过程。 进程将在此过程中分配新的 VM。 生成的 VM 的数量将与原始数量相同，但要对在该过程中创建更多的 VM 做好计划。

-   未规划系统更新或计划的任务。

-   在已使用与生产群集相同的值配置的群集上设置分阶段升级，并在其上测试升级，然后再在生产群集中执行此操作。

### <a name="use-the-upgrade-command"></a>使用升级命令

你需要使用 AKS 引擎的“upgrade”命令，如[升级 Azure Stack Hub 上的 Kubernetes 群集](/azure-stack/user/azure-stack-kubernetes-aks-engine-upgrade)一文所述。

### <a name="upgrade-interruptions"></a>升级中断

有时，意外的因素会中断群集的升级。 当 AKS 引擎报告了某个错误或在 AKS 引擎执行过程中发生某些事情时，可能会发生中断。 请检查中断的原因，解决问题，然后再次提交相同的升级命令以继续执行升级过程。 **upgrade** 命令是幂等的，在重新提交命令后应该会恢复群集的升级。 通常，中断会增加完成更新所需的时间，但不会影响它的完成。

### <a name="estimated-upgrade-time"></a>估计的升级时间

群集中每个 VM 的估计时间为 12 到 15 分钟。 例如，一个 20 节点的群集可能需要大约五 (5) 小时才能完成升级。

## <a name="download-new-image-and-aks-engine"></a>下载新映像和 AKS 引擎

下载 AKS 基础 Ubuntu 映像和 AKS 引擎的新版本。

如 Azure Stack Hub 的 AKS 引擎文档中所述，部署 Kubernetes 群集需要两个主要组件： 
- aks-engine 二进制文件
- AKS 基础 Ubuntu 16.04-LTS 映像发行版

此更新提供了这些组件的新版本：

-   Azure Stack Hub 操作员需要下载新的 AKS 基础 Ubuntu 映像：

    -   名称：`AKS Base Ubuntu 16.04-LTS Image Distro, March 2020`
    -   版本：`2020.03.19`
    -   按照[将 Azure Kubernetes 服务 (AKS) 引擎必备组件添加到 Azure Stack Hub 市场](/azure-stack/operator/azure-stack-aks-engine)一文中的说明进行操作

-   Kubernetes 群集管理员将需要下载新的 aks-engine 版本 0.48.0。 请参阅[在 Linux 上的 Azure Stack Hub 中安装 AKS 引擎](/azure-stack/user/azure-stack-kubernetes-aks-engine-deploy-linux)一文中的说明。 你可以遵循用于首次安装群集的过程。 此更新将覆盖以前的二进制文件。 例如，如果你使用了 `get-akse.sh` 脚本，请按照本文所述步骤[在连接的环境中进行安装](/azure-stack/user/azure-stack-kubernetes-aks-engine-deploy-linux#install-in-a-connected-environment)。 如果你是在 Windows 系统上安装，则此过程同样适用，请参阅[在 Windows 上的 Azure Stack Hub 中安装 AKS 引擎](/azure-stack/user/azure-stack-kubernetes-aks-engine-deploy-windows)一文。

## <a name="kubernetes-version-upgrade-path"></a>Kubernetes 版本升级路径

可以在下表中找到 Azure Stack Hub 的当前版本和升级版本。 不要遵循 aks-engine `get-versions` 命令，因为该命令还包括全局 Azure 中支持的版本。 以下版本和升级表适用于 Azure Stack Hub 中的 AKS 引擎群集。

| **当前版本** | **提供的升级版本** |
| --------------------| ----------------------|
| 1.14.7 | 1.15.10 |
| 1.14.8 | 1.15.10 |
| 1.15.4 | 1.15.10 |
| 1.15.5 | 1.15.10 |

## <a name="whats-new"></a>新增功能 

-   支持 Kubernetes 版本 1.15.10 ([\#2834](https://github.com/Azure/aks-engine/issues/2834))。 部署新群集时，请确保在你的 API 模型 json 文件（即 群集定义文件）中指定发行版本号和次版本号。 你可以找到示例：[kubernetes-azurestack.json](https://raw.githubusercontent.com/Azure/aks-engine/master/examples/azure-stack/kubernetes-azurestack.json)：

    - `"orchestratorRelease": "1.15`,

    - `"orchestratorVersion": "1.15.10"`

    > [!Note]  
    > 如果在 API 模型 json 文件中没有显式地提供 Kubernetes 版本，则将使用版本 `1.15` ([\#2932](https://github.com/Azure/aks-engine/issues/2932))，而 orchestratorVersion 将默认为` 1.15.11`，这将导致在部署群集时出错。

-   对于 aks-engine v0.43.1，云提供程序执行其控制循环和其他任务的默认频率设置不能很好地与 Azure Stack Hub 资源管理器对传入请求的阈值限制配合使用。 此更新将更改 Azure Stack Hub 的默认设置，以减少到 Azure Stack Hub 资源管理器的重试加载 ([\#2861](https://github.com/Azure/aks-engine/issues/2861))。

-   如果 API 模型 json 文件包含 Azure Stack Hub 不支持的属性，则 aks-engine 中的新验证步骤会导致执行停止或显示警告 ([\#2717](https://github.com/Azure/aks-engine/issues/2717))。

-   使用新的验证签入，aks-engine 将验证所执行的 aks-engine 版本所需的 AKS 基础映像版本的可用性 ([\#2342](https://github.com/Azure/aks-engine/issues/2342))。 这将发生在分析 API 模型文件之后和调用 Azure Stack Hub 资源管理器之前。

-   “upgrade”命令中新的 aks-engine 选项“--control-plane-only”允许用户将操作升级为仅针对主虚拟机 ([\#2635](https://github.com/Azure/aks-engine/issues/2635))。

-   对于 Ubuntu 16.04-LTS，已更新到 Linux 内核版本 4.15.0-1071-azure。 有关详细信息，请参阅“[Package: linux-image-4.15.0-1071-azure (4.15.0-1071.76) \[安全性\]](https://packages.ubuntu.com/xenial/linux-image-4.15.0-1071-azure)”。

-   新的 hyperkube 更新支持 Kubernetes 版本 1.14.8 和 1.15.10。

-   更新了 kubectl，使之与群集的 Kubernetes 版本匹配。 此组件在 Kubernetes 群集主节点中提供，通过 SSH 连接到主节点即可运行它。

-   通过最新的 [2020 年 2 月版本](https://github.com/microsoft/Docker-Provider/blob/ci_feature_prod/README.md)更新了 Azure 容器监视器附加产品 ([\#2850](https://github.com/Azure/aks-engine/issues/2850))。

-   将 `coredns` 升级到了版本 v1.6.6 ([\#2555](https://github.com/Azure/aks-engine/issues/2555))。

-   将 `etcd` 升级到了版本 3.3.18 ([\#2462](https://github.com/Azure/aks-engine/issues/2462))。

-   将 `moby` 升级到了版本 3.0.11 ([\#2887](https://github.com/Azure/aks-engine/issues/2887))。

-   使用此版本时，AKS 引擎不再依赖于 `k8s.gcr.io`，现在在构建其映像时将使用官方的 `Kubernetes MCR registry @ mcr.microsoft.com` ([\#2722](https://github.com/Azure/aks-engine/issues/2722))。

## <a name="known-issues"></a>已知问题

-  在单个群集内并行部署多个 Kubernetes 服务可能会导致基本负载均衡器配置错误。 如果可能，请一次部署一个服务。

-  运行 aks-engine get-versions 将生成适用于 Azure 和 Azure Stack Hub 的信息，但是，没有明确的方法来识别与 Azure Stack Hub 对应的内容。 不要使用此命令来确定可升级的版本区域。 请使用上述升级参考表。

-  因为 aks-engine 工具是一个跨 Azure 和 Azure Stack Hub 的共享源代码存储库， 查看许多发行说明和拉取请求会使你相信该工具支持上面所列版本之外的其他版本的 Kubernetes 和 OS 平台。请忽略它们并使用上面的版本表作为此更新的官方指南。

## <a name="reference"></a>参考

下面是一些已修复的 bug 的列表，以及从 0.44.0 到 0.48.0 的一整套发行说明。请注意，后一个列表将包括 Azure 和 Azure Stack Hub 项

### <a name="bug-fixes"></a>Bug 修复

-   Windows `azure.json` 中的 `userAssignedIdentityId` 缺少引号 ([\#2327](https://github.com/Azure/aks-engine/issues/2327))

-   加载项 `update config` 仅限升级 ([\#2282](https://github.com/Azure/aks-engine/issues/2282))

-   升级了在 Windows 节点上获取管理 IP 的超时 ([\#2284](https://github.com/Azure/aks-engine/issues/2284))

-   在 Windows VHD 中添加了 1.0.28 Azure CNI zip 文件 ([\#2268](https://github.com/Azure/aks-engine/issues/2268))

-   更正了设置 IPAddressCount 的默认顺序 ([\#2358](https://github.com/Azure/aks-engine/issues/2358))

-   更新为对所有 k8s 版本使用单个 omsagent yaml 以避免任何人为错误并轻松进行维护 ([\#2692](https://github.com/Azure/aks-engine/issues/2692))

### <a name="release-notes"></a>发行说明

这是组合了 Azure 和 Azure Stack Hub 的发行说明的完整集合

-   https://github.com/Azure/aks-engine/releases/tag/v0.44.0
-   https://github.com/Azure/aks-engine/releases/tag/v0.44.1
-   https://github.com/Azure/aks-engine/releases/tag/v0.44.2
-   https://github.com/Azure/aks-engine/releases/tag/v0.45.0
-   https://github.com/Azure/aks-engine/releases/tag/v0.46.0
-   https://github.com/Azure/aks-engine/releases/tag/v0.46.1
-   https://github.com/Azure/aks-engine/releases/tag/v0.46.2
-   https://github.com/Azure/aks-engine/releases/tag/v0.46.3
-   https://github.com/Azure/aks-engine/releases/tag/v0.47.0
-   https://github.com/Azure/aks-engine/releases/tag/v0.48.0
::: moniker-end
## <a name="next-steps"></a>后续步骤

- 了解 [Azure Stack Hub 上的 AKS 引擎](azure-stack-kubernetes-aks-engine-overview.md)
