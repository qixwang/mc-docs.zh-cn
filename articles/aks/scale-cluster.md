---
title: 缩放 Azure Kubernetes 服务 (AKS) 群集
description: 了解如何在 Azure Kubernetes 服务 (AKS) 群集中缩放节点数。
services: container-service
author: rockboyfor
ms.topic: article
origin.date: 05/31/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: 7e834c2cf1a3ee24a8d670311d6a59ee880b5caf
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290713"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>在 Azure Kubernetes 服务 (AKS) 群集中缩放节点数

如果资源需要更改应用程序，可以手动缩放 AKS 群集以运行不同数量的节点。 节点数减少时，节点会被仔细[封锁和排除][kubernetes-drain]，尽量避免对正在运行的应用程序造成中断。 纵向扩展时，AKS 会一直等待，直到节点被 Kubernetes 群集标记为 `Ready`，然后才在这些节点上计划 Pod。

## <a name="scale-the-cluster-nodes"></a>缩放群集节点

首先，使用 [az aks show][az-aks-show] 命令获取节点池的名称  。 以下示例获取 myResourceGroup 资源组中名为 myAKSCluster 的群集的节点池名称   ：

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles
```

以下示例输出表明名称为 nodepool1   ：

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles

[
  {
    "count": 1,
    "maxPods": 110,
    "name": "nodepool1",
    "osDiskSizeGb": 30,
    "osType": "Linux",
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_DS2_v2"
  }
]
```

使用 [az aks scale][az-aks-scale] 命令缩放群集节点。 以下示例将名为 *myAKSCluster* 的群集缩放为单个节点。 提供前一个命令中自己的 --nodepool-name，如 nodepool1   ：

```azurecli
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 1 --nodepool-name <your node pool name>
```

以下示例输出显示群集已成功缩放为一个节点，如 agentPoolProfiles 部分中所示  ：

```json
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "count": 1,
      "maxPods": 110,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  [...]
}
```

## <a name="next-steps"></a>后续步骤

在本文中，你手动缩放了 AKS 群集以增加或减少节点数量。 

<!--Not Available on [cluster autoscaler][cluster-autoscaler](currently in preview in AKS)-->
<!-- LINKS - external -->

[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->

[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-show
[az-aks-scale]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-scale

<!--Not Available on [cluster-autoscaler]: cluster-autoscaler.md-->
<!-- Update_Description: wording update, update link -->