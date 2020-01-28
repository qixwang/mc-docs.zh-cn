---
title: 在 Azure Kubernetes 服务 (AKS) 中使用多个节点池
description: 了解如何为 Azure Kubernetes 服务 (AKS) 中的群集创建和管理多个节点池
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 12/16/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 72bd80ee0b78fd25adb1961c2f2ab83918038126
ms.sourcegitcommit: de60969043e6dd8ef706ed13e0684a7c35b26bdb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2020
ms.locfileid: "76550221"
---
# <a name="create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>为 Azure Kubernetes 服务 (AKS) 中的群集创建和管理多个节点池

在 Azure Kubernetes 服务 (AKS) 中，采用相同配置的节点分组成节点池。  这些节点池包含运行应用程序的底层 VM。 节点的初始数量和大小 (SKU) 是创建 AKS 群集时（此时会创建默认节点池）定义的。  若要支持具有不同计算或存储需求的应用程序，可以创建额外的节点池。 例如，使用这些额外节点池可为计算密集型应用程序提供 GPU，或访问高性能 SSD 存储。

> [!NOTE]
> 利用此功能可以更好地控制如何创建和管理多个节点池。 因此，需要使用单独的命令来执行创建/更新/删除操作。 以前，通过 `az aks create` 或 `az aks update` 执行的群集操作使用 managedCluster API，并且只能通过这些操作更改控制平面和单个节点池。 此功能通过 agentPool API 为代理池公开单独的操作集，并要求使用 `az aks nodepool` 命令集对单个节点池执行操作。

本文介绍如何在 AKS 群集中创建和管理多个节点池。

## <a name="before-you-begin"></a>准备阶段

需要安装并配置 Azure CLI 2.0.76 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI][install-azure-cli]。

## <a name="limitations"></a>限制

创建和管理支持多个节点池的 AKS 群集时存在以下限制：

* 无法删除默认的（第一个）节点池。
* 无法使用 HTTP 应用程序路由加载项。
* AKS 群集必须通过标准 SKU 负载均衡器来使用多个节点池，而基本 SKU 负载均衡器并不支持该功能。
* AKS 群集必须对节点使用虚拟机规模集。
* 节点池的名称只能包含小写字母数字字符，且必须以小写字母开头。 Linux 节点池的名称长度必须为 1 到 12 个字符；Windows 节点池的名称长度必须为 1 到 6 个字符。
* AKS 群集最多可以包含 8 个节点池。
* AKS 群集在这八节点池中最多可以有 800 个节点。
* 所有节点池必须位于同一 vnet 和子网中。
* 在创建群集的过程中创建多个节点池时，节点池使用的所有 Kubernetes 版本都必须与已为控制平面设置的版本相匹配。 这可以在已使用每节点池操作预配了群集后更新。

## <a name="create-an-aks-cluster"></a>创建 AKS 群集

若要开始，请创建包含单个节点池的 AKS 群集。 以下示例使用 [az group create][az-group-create] 命令在 *chinaeast2* 区域中创建名为 *myResourceGroup* 的资源组。 然后使用 [az AKS create][az-aks-create] 命令创建名为 *myAKSCluster* 的 AKS 群集。 值为 *1.13.10* 的 *--kubernetes-version* 用于演示如何在以下步骤中更新节点池。 可以指定任何[支持的 Kubernetes 版本][supported-versions]。

> [!NOTE]
> 使用多个节点池时，**不支持**“基本”负载均衡器 SKU。  默认情况下，AKS 群集是在 Azure CLI 和 Azure 门户中使用“标准”负载均衡器 SKU 创建的。 

```azurecli
# Create a resource group in China East 2
az group create --name myResourceGroup --location chinaeast2

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 2 \
    --generate-ssh-keys \
    --kubernetes-version 1.13.10 \
    --load-balancer-sku standard
```

创建群集需要几分钟时间。

> [!NOTE]
> 为了确保群集可靠运行，应在默认节点池中至少运行 2（两）个节点，因为基本系统服务将在整个节点池中运行。

群集准备就绪后，请使用 [az aks get-credentials][az-aks-get-credentials] 命令获取要与 `kubectl` 结合使用的群集凭据：

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>添加节点池

在上一步骤中创建的群集包含单个节点池。 让我们使用 [az aks nodepool add][az-aks-nodepool-add] 命令添加另一个节点池。 以下示例创建名为 *mynodepool* 的节点池，该节点池运行 *3* 个节点：

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3 \
    --kubernetes-version 1.12.7
```

> [!NOTE]
> 节点池的名称必须以小写字母开头，且只能包含字母数字字符。 Linux 节点池的名称长度必须为 1 到 12 个字符；Windows 节点池的名称长度必须为 1 到 6 个字符。

若要查看节点池的状态，请使用 [az aks node pool list][az-aks-nodepool-list] 命令并指定资源组和群集名称：

```azurecli
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster
```

以下示例输出显示已成功地在节点池中创建包含三个节点的 *mynodepool*。 在上一步骤中创建 AKS 群集时，已创建包含 *2* 个节点的默认 *nodepool1*。

```console
$ az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.12.7",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

> [!TIP]
> 如果在添加节点池时未指定 *OrchestratorVersion* 或 *VmSize*，则会根据 AKS 群集的默认值创建节点。 在此示例中，Kubernetes 版本为 *1.13.10*，节点大小为 *Standard_DS2_v2*。

## <a name="upgrade-a-node-pool"></a>升级节点池

> [!NOTE]
> 不能对群集或节点池同时执行升级和缩放操作，否则会返回错误。 而只能先在目标资源上完成一个操作类型，然后再在同一资源上执行下一个请求。 请阅读[故障排除指南](https://docs.azure.cn/aks/troubleshooting#im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade)中的详细信息。

最初在第一个步骤中创建 AKS 群集时，为 `--kubernetes-version` 指定了 *1.13.10*。 这同时设置了控制平面和默认节点池的 Kubernetes 版本。 本部分中的命令说明如何升级单个特定的节点池。

[下一部分](#upgrade-a-cluster-control-plane-with-multiple-node-pools)将会说明升级控制平面与节点池的 Kubernetes 版本之间的关系。

> [!NOTE]
> 节点池的 OS 映像版本与群集的 Kubernetes 版本相关联。 只能先升级 OS 映像，然后再升级群集。

由于本示例包含两个节点池，因此必须使用 [az aks nodepool upgrade][az-aks-nodepool-upgrade] 来升级节点池。 让我们将 *mynodepool* 升级到 Kubernetes *1.13.10*。 如以下示例中所示，使用 [az aks nodepool upgrade][az-aks-nodepool-upgrade] 命令升级节点池：

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.13.10 \
    --no-wait
```

使用 [az aks node pool list][az-aks-nodepool-list] 命令再次列出节点池的状态。 以下示例显示 *mynodepool* 的状态为“正在升级”到 *1.13.10*： 

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Upgrading",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

将节点升级到指定的版本需要花费几分钟时间。

最佳做法是将 AKS 群集中的所有节点池升级到相同的 Kubernetes 版本。 `az aks upgrade` 的默认行为是将所有节点池连同控制平面一起升级，以实现这种一致性。 升级单个节点池的功能可让你执行滚动升级，并在节点池之间计划 pod，使应用程序的正常运行时间保持在上述约束范围内。

## <a name="upgrade-a-cluster-control-plane-with-multiple-node-pools"></a>升级包含多个节点池的群集控制平面

> [!NOTE]
> Kubernetes 使用标准的[语义化版本控制](https://semver.org/)方案。 版本号以 *x.y.z* 表示，其中，*x* 是主要版本，*y* 是次要版本，*z* 是修补程序版本。 例如，版本 *1.12.6* 中的 1 是主要版本，12 是次要版本，6 是修补程序版本。 控制平面和初始节点池的 Kubernetes 版本是在群集创建过程中设置的。 将所有附加节点池添加到群集时，将为其设置 Kubernetes 版本。 不同节点池的 Kubernetes 版本可能不同，节点池与控制平面的 Kubernetes 版本也可能不同。

AKS 群集包含两个具有关联 Kubernetes 版本的群集资源对象。

1. 群集控制平面 Kubernetes 版本。
2. 具有 Kubernetes 版本的节点池。

一个控制平面映射到一个或多个节点池。 升级操作的行为取决于所用的 Azure CLI 命令。

升级 AKS 控制平面需要使用 `az aks upgrade`。 这会升级群集中的控制平面版本和所有节点池。 

结合 `--control-plane-only` 标志发出 `az aks upgrade` 命令只会升级群集控制平面， 而不会更改群集中任何关联的节点池。

升级单个节点池需要使用 `az aks nodepool upgrade`。 这只会升级具有指定 Kubernetes 版本的目标节点池

### <a name="validation-rules-for-upgrades"></a>升级验证规则

群集控制平面和节点池的有效 Kubernetes 升级由以下规则集验证。

* 用于升级节点池的有效版本的规则：
   * 节点池版本的主要版本必须与控制平面相同。 
   * 节点池的次要版本必须在控制平面版本的两个次要版本范围内。  
   * 节点池版本不能大于控制平面的 `major.minor.patch` 版本。

* 提交升级操作的规则：
   * 无法降级控制平面或节点池的 Kubernetes 版本。
   * 如果未指定节点池的 Kubernetes 版本，则行为取决于所用的客户端。 资源管理器模板中的声明将回退到为节点池定义的现有版本（如果使用），如果未设置现有版本，将使用控制平面版本进行回退。
   * 可以在给定的时间升级或者缩放控制平面或节点池，而不能同时对单个控制平面或节点池资源提交多个操作。

## <a name="scale-a-node-pool-manually"></a>手动缩放节点池

当应用程序工作负荷需求发生变化时，你可能需要缩放节点池中的节点数。 可以增加或减少节点数。

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

若要缩放节点池中的节点数，请使用 [az aks node pool scale][az-aks-nodepool-scale] 命令。 以下示例将 *mynodepool* 中的节点数缩放为 *5*：

```azurecli
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

使用 [az aks node pool list][az-aks-nodepool-list] 命令再次列出节点池的状态。 以下示例显示 *mynodepool* 处于 *Scaling*（正在缩放）状态，其中的新节点计数为 *5*：

```console
$ az aks nodepool list -g myResourceGroupPools --cluster-name myAKSCluster

[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Scaling",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

需要花费几分钟时间来完成缩放操作。

<!--Not Available on ## Scale a specific node pool automatically by enabling the cluster autoscaler-->
<!--Not Available on [cluster autoscaler](cluster-autoscaler.md)-->
<!--Not Available on [use the cluster autoscaler per node pool](cluster-autoscaler.md#use-the-cluster-autoscaler-with-multiple-node-pools-enabled)-->

## <a name="delete-a-node-pool"></a>删除节点池

不再需要某个池时，可将其删除并删除底层 VM 节点。 若要删除节点池，请使用 [az aks node pool delete][az-aks-nodepool-delete] 命令并指定节点池名称。 以下示例删除在前面步骤中创建的 *mynoodepool*：

> [!CAUTION]
> 如果删除节点池时发生数据丢失，没有任何选项可以恢复数据。 如果无法在其他节点池中计划 pod，则这些应用程序将不可用。 当使用中的应用程序没有数据备份或者无法在群集中的其他节点池上运行时，请务必不要删除节点池。

```azurecli
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

[az aks node pool list][az-aks-nodepool-list] 命令的以下示例输出显示 *mynodepool* 处于 *Deleting*（正在删除）状态：

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Deleting",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

删除节点和节点池需要花费几分钟时间。

## <a name="specify-a-vm-size-for-a-node-pool"></a>指定节点池的 VM 大小

在前面的创建节点池示例中，对群集中创建的节点使用了默认 VM 大小。 一个更常见的场景是创建具有不同 VM 大小和功能的节点池。 例如，可以创建一个包含具有大量 CPU 或内存的节点的节点池，或创建一个提供 GPU 支持的节点池。 下一步骤将[使用排斥和容许](#schedule-pods-using-taints-and-tolerations)来告知 Kubernetes 计划程序如何将访问权限限制为可在这些节点上运行的 pod。

以下示例创建使用 *Standard_NC6s_v3* VM 大小的基于 GPU 的节点池。 这些 VM 采用 NVIDIA Tesla K80 卡。 有关可用 VM 大小的信息，请参阅 [Azure 中的 Linux 虚拟机大小][vm-sizes]。

再次使用 [az aks node pool add][az-aks-nodepool-add] 命令创建节点池。 这一次请指定名称 *gpunodepool*，并使用 `--node-vm-size` 参数指定 *Standard_NC6s_v3* 大小：

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6s_v3 \
    --no-wait
```

[az aks node pool list ][az-aks-nodepool-list] 命令的以下示例输出显示 *gpunodepool* 正在创建具有指定 *VmSize* 的节点： 

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 1,
    ...
    "name": "gpunodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Creating",
    ...
    "vmSize": "Standard_NC6s_v3",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

成功创建 *gpunodepool* 需要花费几分钟时间。

## <a name="schedule-pods-using-taints-and-tolerations"></a>使用排斥和容许计划 pod

群集现在包含两个节点池 - 最初创建的默认节点池，以及基于 GPU 的节点池。 使用 [kubectl get nodes][kubectl-get] 命令查看群集中的节点。 以下示例输出显示了节点：

```console
$ kubectl get nodes

NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.13.10
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.13.10
```

Kubernetes 计划程序能够使用排斥和容许来限制可在节点上运行的工作负荷。

* 将**排斥**应用到指明了只能计划特定 pod 的节点。
* 然后，将**容许**应用到可以*容许*节点排斥的 pod。

有关如何使用 Kubernetes 高级计划功能的详细信息，请参阅[有关 AKS 中的高级计划程序功能的最佳做法][taints-tolerations]

本示例使用 [kubectl taint node][kubectl-taint] 命令向基于 GPU 的节点应用排斥。 指定上述 `kubectl get nodes` 命令的输出中显示的基于 GPU 的节点名称。 排斥以“键:值”的形式应用，后接计划选项。  以下示例使用 *sku=gpu* 对，并定义具有 *NoSchedule* 功能的其他 pod：

```console
kubectl taint node aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

以下基本示例 YAML 清单使用容许来允许 Kubernetes 计划程序在基于 GPU 的节点上运行 NGINX pod。 有关针对 MNIST 数据集运行 Tensorflow 作业的更适当但更耗时的示例，请参阅[对 AKS 上的计算密集型工作负荷使用 GPU][gpu-cluster]。

创建名为 `gpu-toleration.yaml` 的文件，并将其复制到以下示例 YAML 中：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: dockerhub.azk8s.cn/library/nginx:1.15.9
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

使用 `kubectl apply -f gpu-toleration.yaml` 命令计划 pod：

```console
kubectl apply -f gpu-toleration.yaml
```

只需花费几秒钟时间即可计划 pod 并提取 NGINX 映像。 使用 [kubectl describe pod][kubectl-describe] 命令查看 pod 状态。 以下精简示例输出显示已应用 *sku=gpu:NoSchedule* 容许。 在 events 节中，计划程序已将 pod 分配到 *aks-gpunodepool-28993262-vmss000000* 基于 GPU 的节点：

```console
$ kubectl describe pod mypod

[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                                          Message
  ----    ------     ----   ----                                          -------
  Normal  Scheduled  4m48s  default-scheduler                             Successfully assigned default/mypod to aks-gpunodepool-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet, aks-gpunodepool-28993262-vmss000000  pulling image "nginx:1.15.9"
  Normal  Pulled     4m43s  kubelet, aks-gpunodepool-28993262-vmss000000  Successfully pulled image "nginx:1.15.9"
  Normal  Created    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Created container
  Normal  Started    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Started container
```

只能在 *gpunodepool* 中的节点上计划已应用此排斥的 pod。 任何其他 pod 将在 *nodepool1* 节点池中计划。 如果创建额外的节点池，可以使用额外的排斥和容许来限制可在这些节点资源上计划的 pod。

## <a name="manage-node-pools-using-a-resource-manager-template"></a>使用资源管理器模板管理节点池

使用 Azure 资源管理器模板创建和管理资源时，通常可以更新该模板中的设置，然后重新部署以更新资源。 对于 AKS 中的节点池，一旦创建 AKS 群集，就无法更新初始节点池的配置文件。 此行为意味着无法更新现有的资源管理器模板，更改节点池，然后重新部署。 必须创建单独的资源管理器模板来仅更新现有 AKS 群集的节点池。

创建一个模板（例如 `aks-agentpools.json`）并粘贴以下示例清单。 此示例模板配置以下设置：

* 将名为 *myagentpool* 的 *Linux* 节点池更新为运行三个节点。
* 将节点池中的节点设置为运行 Kubernetes 版本 *1.13.10*。
* 将节点大小定义为 *Standard_DS2_v2*。

根据需要编辑这些值，以更新、添加或删除节点池：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "The name of your existing AKS cluster."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "The location of your existing AKS cluster."
            }
        },
        "agentPoolName": {
            "type": "string",
            "defaultValue": "myagentpool",
            "metadata": {
                "description": "The name of the agent pool to create or update."
            }
        },
        "vnetSubnetId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The Vnet subnet resource ID for your existing AKS cluster."
            }
        }
    },
    "variables": {
        "apiVersion": {
            "aks": "2019-04-01"
        },
        "agentPoolProfiles": {
            "maxPods": 30,
            "osDiskSizeGB": 0,
            "agentCount": 3,
            "agentVmSize": "Standard_DS2_v2",
            "osType": "Linux",
            "vnetSubnetId": "[parameters('vnetSubnetId')]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.ContainerService/managedClusters/agentPools",
            "name": "[concat(parameters('clusterName'),'/', parameters('agentPoolName'))]",
            "location": "[parameters('location')]",
            "properties": {
                "maxPods": "[variables('agentPoolProfiles').maxPods]",
                "osDiskSizeGB": "[variables('agentPoolProfiles').osDiskSizeGB]",
                "count": "[variables('agentPoolProfiles').agentCount]",
                "vmSize": "[variables('agentPoolProfiles').agentVmSize]",
                "osType": "[variables('agentPoolProfiles').osType]",
                "storageProfile": "ManagedDisks",
                "type": "VirtualMachineScaleSets",
                "vnetSubnetID": "[variables('agentPoolProfiles').vnetSubnetId]",
                "orchestratorVersion": "1.13.10"
            }
        }
    ]
}
```

如以下示例中所示，使用 [az group deployment create][az-group-deployment-create] 命令部署此模板。 系统将提示输入现有的 AKS 群集名称和位置：

```azurecli
az group deployment create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

更新 AKS 群集可能需要花费几分钟时间，具体取决于资源管理器模板中定义的节点池设置和操作。

## <a name="assign-a-public-ip-per-node-in-a-node-pool"></a>为节点池中的每个节点分配公共 IP

> [!WARNING]
> 在预览版中为每个节点分配公共 IP 期间，不能对 AKS 中的标准负载均衡器 SKU 使用此操作，因为负载均衡器规则可能与 VM 预配相冲突。  在预览版中，如果需要为每个节点分配公共 IP，必须使用基本负载均衡器 SKU。 

AKS 节点无需使用自身的公共 IP 地址进行通信。 但某些情况下，节点池中的节点可能需要自身的公共 IP 地址。 例如，在玩游戏时，控制台需要直接连接到云虚拟机才能尽量减少画面跳跃。 为此，可以注册单独的预览版功能“节点公共 IP”。

```azurecli
az feature register --name NodePublicIPPreview --namespace Microsoft.ContainerService
```

成功注册后，根据[上述](#manage-node-pools-using-a-resource-manager-template)说明部署 Azure 资源管理器模板，并将布尔值属性 `enableNodePublicIP` 添加到 agentPoolProfiles。 将值设置为 `true`；如果未指定，它将默认设置为 `false`。 此属性只在创建时起作用，所需的最低 API 版本为 2019-06-01。 此属性可同时应用到 Linux 和 Windows 节点池。

## <a name="clean-up-resources"></a>清理资源

在本文中，你已创建包含基于 GPU 的节点的 AKS 群集。 为了减少不必要的费用，我们建议删除 *gpunodepool* 或整个 AKS 群集。

若要删除基于 GPU 的节点池，请如以下示例中所示使用 [az aks nodepool delete][az-aks-nodepool-delete] 命令：

```azurecli
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

若要删除群集本身，请使用 [az group delete][az-group-delete] 命令删除 AKS 资源组：

```azurecli
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>后续步骤

本文已介绍如何在 AKS 群集中创建和管理多个节点池。 有关如何跨节点池控制 pod 的详细信息，请参阅[有关 AKS 中的高级计划程序功能的最佳做法][operator-best-practices-advanced-scheduler]。

<!--Not Available on [Create a Windows Server container in AKS][aks-windows]-->

<!-- EXTERNAL LINKS -->

[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- INTERNAL LINKS -->

[az-aks-get-credentials]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create
[az-aks-create]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-nodepool-add]: https://docs.azure.cn/cli/ext/aks-preview/aks/nodepool?view=azure-cli-latest#ext-aks-preview-az-aks-nodepool-add
[az-aks-nodepool-list]: https://docs.azure.cn/cli/ext/aks-preview/aks/nodepool?view=azure-cli-latest#ext-aks-preview-az-aks-nodepool-list
[az-aks-nodepool-upgrade]: https://docs.azure.cn/cli/ext/aks-preview/aks/nodepool?view=azure-cli-latest#ext-aks-preview-az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: https://docs.azure.cn/cli/ext/aks-preview/aks/nodepool?view=azure-cli-latest#ext-aks-preview-az-aks-nodepool-scale
[az-aks-nodepool-delete]: https://docs.azure.cn/cli/ext/aks-preview/aks/nodepool?view=azure-cli-latest#ext-aks-preview-az-aks-nodepool-delete
[vm-sizes]: ../virtual-machines/linux/sizes.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[gpu-cluster]: gpu-cluster.md
[az-group-delete]: https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete
[install-azure-cli]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest
[supported-versions]: supported-kubernetes-versions.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md

<!--Not Available on [aks-windows]: windows-container-cli.md-->
    
[az-group-deployment-create]: https://docs.azure.cn/cli/group/deployment?view=azure-cli-latest#az-group-deployment-create


<!-- Update_Description: update meta properties, wording update, update link -->