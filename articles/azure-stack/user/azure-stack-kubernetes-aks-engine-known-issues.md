---
title: 在 Azure Stack Hub 上使用 AKS 引擎的已知问题
description: 了解在 Azure Stack Hub 上使用 AKS 引擎的已知问题。
author: WenJason
ms.topic: article
origin.date: 07/02/2020
ms.date: 07/20/2020
ms.author: v-jay
ms.reviewer: waltero
ms.lastreviewed: 07/02/2020
ms.openlocfilehash: a7f7903c82836de84f473ac84fb47c2d2f4a0df1
ms.sourcegitcommit: e9ffd50aa5eaab402a94bfabfc70de6967fe6278
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86307758"
---
# <a name="known-issues-with-the-aks-engine-on-azure-stack-hub"></a>在 Azure Stack Hub 上使用 AKS 引擎的已知问题

本主题介绍 Azure Stack Hub 上的 AKS 引擎的已知问题。

## <a name="upgrade-issues-in-aks-engine-0510"></a>AKS 引擎 0.51.0 中的升级问题

* 在将 Kubernetes 群集从 1.15.x 版升级到 1.16.x 版（aks 引擎升级）期间，以下 Kubernetes 组件的升级需要额外的手动步骤：kube-proxy、azure-cni-networkmonitor、csi-secrets-store、kubernetes-dashboard。 下面描述了你可能观察到的情况以及如何解决这些问题。

  * 在已连接的环境中，由于群集中没有迹象表明受影响的组件未升级，因此不会明显注意到此问题。 一切似乎都按预期进行。
  * 在离线环境中，当你运行查询系统 Pod 状态的查询并看到下面提到的组件的 Pod 未处于“Ready”状态时，可以发现此问题：

    ```PowerShell
    kubectl get pods -n kube-system
    ```

  * 若要为上述每个组件解决此问题，请运行下表的“解决方法”列中的命令。

    |组件名称 |解决方法 |受影响方案|
    |---------------|-----------|------------------|
    |kube-proxy     | `kubectl delete ds kube-proxy -n kube-system` |已连接、离线 |
    |azure-cni-networkmonitor   | `kubectl delete ds azure-cni-networkmonitor -n kube-system`   | 已连接、离线 |
    |csi-secrets-store  |`sudo sed -i s/Always/IfNotPresent/g /etc/kubernetes/addons/secrets-store-csi-driver.yaml`<br>`kubectl delete ds csi-secrets-store -n kube-system` | 已断开连接 |
    |kubernetes-dashboard |在每个主节点上运行以下命令：<br>`sudo sed -i s/Always/IfNotPresent/g /etc/kubernetes/addons/kubernetes-dashboard.yaml` |已断开连接 |

* 此版本不支持 Kubernetes 1.17。 尽管有 PR 暗示它，但 1.17 不受支持。

## <a name="basic-load-balancer-sku-limitations"></a>基本负载均衡器 SKU 限制

* 单一代理池限制。 目前，Azure Stack Hub 仅支持基本负载均衡器 SKU。 此 SKU 将后端池终结点[限制](/load-balancer/concepts-limitations#skus)为单个可用性集（或虚拟机规模集）中的虚拟机。 这意味着，LoadBalancer 服务的所有副本都应部署在同一代理池中，同时也意味着每个单独的群集都可以有一个 Linux LoadBalancer 服务或 Windows LoadBalancer 服务。

  可以通过在 Pod 模板中添加以下项来强制 Kubernetes 在特定的代理池中创建 Pod：[节点选择器](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)“agentpool:MY_POOL_NAME”。

  ```powershell
  nodeSelector:

        agentpool: linuxpool
  ```
  
  如果已在群集中创建了一个 LoadBalancer 服务，则可以通过在 Azure Stack Hub 门户中检查负载均衡器后端池边栏选项卡，了解已选择哪个代理池作为负载均衡器的后端池。 获得该信息后，可以通过更新部署/pod yaml（如上一段所述）来指定目标代理池。

* `get-versions` 的命令范围。 `get-versions` 命令的输出仅适用于 Azure，不适用于 Azure Stack Hub 云。 有关不同升级路径的详细信息，请参阅[升级到更新 Kubernetes 版本的步骤](azure-stack-kubernetes-aks-engine-upgrade.md#steps-to-upgrade-to-a-newer-kubernetes-version)。

## <a name="next-steps"></a>后续步骤

[Azure Stack Hub 上的 AKS 引擎概述](azure-stack-kubernetes-aks-engine-overview.md)
