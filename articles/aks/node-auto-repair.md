---
title: 自动修复 Azure Kubernetes 服务 (AKS) 节点
description: 了解节点自动修复功能，以及 AKS 如何修复损坏的工作器节点。
services: container-service
ms.topic: conceptual
origin.date: 03/10/2020
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: a398831b7b2488282e2f9e130531065f7b3fad7a
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002258"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Azure Kubernetes 服务 (AKS) 节点自动修复

AKS 会持续检查工作器节点的运行状况，在节点运行不正常的情况下对其进行自动修复。 本文档介绍 Azure Kubernetes 服务 (AKS) 如何监视工作器节点，以及如何修复运行不正常的工作器节点。  本文档用于通知 AKS 操作员节点修复功能的行为。 还需要注意的是，Azure 平台会[对遇到问题的虚拟机执行维护][vm-updates]。 AKS 和 Azure 协同工作，以最大限度地减少群集的服务中断次数。

<!--Not Available on  Windows Server node pools-->

## <a name="how-aks-checks-for-unhealthy-nodes"></a>AKS 如何检查运行不正常的节点

> [!Note]
> AKS 使用用户帐户 aks-remediator  对节点执行修复操作。

AKS 使用规则来确定节点是否处于不正常状态以及是否需要修复。 AKS 使用以下规则来确定是否需要自动修复。

* 节点在 10 分钟的时间范围内持续检查时报告状态为“未就绪” 
* 节点在 10 分钟内未报告状态

可使用 kubectl 手动检查节点的运行状况状态。 

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>自动修复的工作原理

> [!Note]
> AKS 使用用户帐户 aks-remediator  对节点执行修复操作。

此行为适用于虚拟机规模集  。  自动修复执行几个步骤来修复已损坏的节点。  如果某个节点确定为运行不正常，AKS 会尝试执行多个修正步骤。  按以下顺序执行这些步骤：

1. 容器运行时 10 分钟无响应后，失败的运行时服务会在节点上重启。
2. 如果节点在 10 分钟内未准备就绪，则会重启该节点。
3. 如果节点在 30 分钟内未准备就绪，则会重置该节点的映像。

> [!Note]
> 如果多个节点运行不正常，则会逐个修复这些节点

<!--Not Avaiable on ## Next steps-->

<!--Not Avaiable on [Availability Zones][availability-zones]-->

<!-- LINKS - External -->

<!-- LINKS - Internal -->

<!--Not Avaiable on [availability-zones]: ./availability-zones.md-->

[vm-updates]: ../virtual-machines/maintenance-and-updates.md

<!-- Update_Description: new article about node auto repair -->
<!--NEW.date: 05/06/2020-->