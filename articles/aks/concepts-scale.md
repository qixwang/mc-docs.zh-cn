---
title: 概念 - 在 Azure Kubernetes 服务 (AKS) 中缩放应用程序
description: 了解 Azure Kubernetes 服务 (AKS) 中的缩放，包括水平 Pod 自动缩放程序、群集自动缩放程序和 Azure 容器实例连接器。
services: container-service
author: rockboyfor
ms.topic: conceptual
origin.date: 02/28/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: 166508c22c33f94120ede728adbfa87ad589352a
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290763"
---
# <a name="scaling-options-for-applications-in-azure-kubernetes-service-aks"></a>Azure Kubernetes 服务 (AKS) 中的应用程序缩放选项

在 Azure Kubernetes 服务 (AKS) 中运行应用程序时，可能需要增加或减少计算资源量。 随着所需应用程序实例数量的变化，可能还需要更改基础 Kubernetes 节点的数量。 可能还需要快速预配大量其他应用程序实例。

本文介绍有助于在 AKS 中缩放应用程序的核心概念：

- [手动缩放](#manually-scale-pods-or-nodes)
- [水平 Pod 自动缩放程序 (HPA)](#horizontal-pod-autoscaler)

<!--Not Available on - [Cluster autoscaler](#cluster-autoscaler)-->
<!--Not Available on - [Azure Container Instance (ACI) integration with AKS](#burst-to-azure-container-instances)-->

## <a name="manually-scale-pods-or-nodes"></a>手动缩放 Pod 或节点

可以手动缩放副本 (Pod) 和节点，以测试应用程序如何响应可用资源和状态的更改。 手动缩放资源还可以定义用于维持固定成本的设定数量的资源，例如节点数。 若要手动缩放，请定义副本或节点计数。 然后，Kubernetes API 根据该副本或节点计数计划创建其他 Pod 或排空节点。

缩减节点时，Kubernetes API 将调用与群集使用的计算类型绑定的相关 Azure 计算 API。 例如，对于基于 VM 规模集构建的群集，选择要删除的节点的逻辑由 VM 规模集 API 确定。 若要详细了解如何在缩减节点时选择要删除的节点，请参阅 [VMSS 常见问题解答](/virtual-machine-scale-sets/virtual-machine-scale-sets-faq#if-i-reduce-my-scale-set-capacity-from-20-to-15-which-vms-are-removed)。

若要开始使用手动缩放 Pod 和节点，请参阅[在 AKS 中缩放应用程序][aks-scale]。

## <a name="horizontal-pod-autoscaler"></a>水平 Pod 自动缩放程序

Kubernetes 使用水平 Pod 自动缩放程序 (HPA) 来监视资源需求并自动缩放副本数量。 默认情况下，水平 Pod 自动缩放程序每隔 30 秒检查一次指标 API，以了解副本计数所需的任何更改。 当需要进行更改时，副本的数量会相应增加或减少。 水平 Pod 自动缩放程序与已经为 Kubernetes 1.8+ 部署了指标服务器的 AKS 群集配合使用。

![Kubernetes 水平 Pod 自动缩放](media/concepts-scale/horizontal-pod-autoscaling.png)

为给定部署配置水平 Pod 自动缩放程序时，请定义可运行的最小和最大副本数。 还可以定义指标以监视任何缩放决策并以此为依据，例如 CPU 使用情况。

若要开始使用 AKS 中的水平 Pod 自动缩放程序，请参阅[在 AKS 中自动缩放 Pod][aks-hpa]。

### <a name="cooldown-of-scaling-events"></a>缩放事件的冷却时间

由于水平 Pod 自动缩放程序每 30 秒检查一次指标 API，因此在进行另一次检查之前，先前的缩放事件可能尚未成功完成。 此行为可能导致水平 Pod 自动缩放程序会在上一个缩放事件能够接收应用程序工作负荷且需要对资源进行相应调整之前更改副本数。

为最大限度地减少这些争用事件，可以设置冷却时间值或延迟值。 这些值定义水平 Pod 自动缩放程序在执行一个缩放事件之后，触发另一个缩放事件之前必须等待的时间。 此行为允许新副本计数生效，指标 API 反映分布式工作负荷。 默认情况下，纵向扩展事件的延迟为 3 分钟，纵向缩减事件的延迟为 5 分钟

目前，无法从默认值调整这些冷却时间值。

<!--Not Available on ## Cluster autoscaler-->

<!--Not Available on ### Scale up events-->
<!--Not Available on ### Scale down events-->
<!--Not Available on ## Burst to Azure Container Instances-->

## <a name="next-steps"></a>后续步骤

若要开始缩放应用程序，请首先按照[使用 Azure CLI 创建 AKS 群集的快速入门][aks-quickstart]进行操作。 然后，可以开始手动或自动缩放 AKS 群集中的应用程序：

- 手动缩放 [Pod][aks-manually-scale-pods] 或[节点][aks-manually-scale-nodes]
- 使用[水平 Pod 自动缩放程序][aks-hpa]

<!--Not Avaialble on - Use the [cluster autoscaler][aks-cluster-autoscaler]-->

有关核心 Kubernetes 和 AKS 概念的详细信息，请参阅以下文章：

- [Kubernetes/AKS 群集和工作负荷][aks-concepts-clusters-workloads]
- [Kubernetes/AKS 访问和标识][aks-concepts-identity]
- [Kubernetes/AKS 安全性][aks-concepts-security]
- [Kubernetes/AKS 虚拟网络][aks-concepts-network]
- [Kubernetes/AKS 存储][aks-concepts-storage]

<!-- LINKS - external -->

[virtual-kubelet]: https://virtual-kubelet.io/

<!-- LINKS - internal -->

[aks-quickstart]: kubernetes-walkthrough.md
[aks-hpa]: tutorial-kubernetes-scale.md#autoscale-pods
[aks-scale]: tutorial-kubernetes-scale.md
[aks-manually-scale-pods]: tutorial-kubernetes-scale.md#manually-scale-pods
[aks-manually-scale-nodes]: tutorial-kubernetes-scale.md#manually-scale-aks-nodes

<!--Not Available on [aks-cluster-autoscaler]: autoscaler.md-->

[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-network]: concepts-network.md

<!--Not Available on [virtual-nodes-cli]: virtual-nodes-cli.md-->

<!-- Update_Description: update meta properties, wording update, update link -->