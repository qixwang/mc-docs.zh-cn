---
title: 自动修复 Azure Kubernetes 服务 (AKS) 节点
description: 了解节点自动修复功能，以及 AKS 如何修复损坏的工作器节点。
services: container-service
ms.topic: conceptual
origin.date: 06/02/2020
ms.date: 07/13/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 80629740e6ec3662e102094b9ac036ecc86c99b4
ms.sourcegitcommit: 6c9e5b3292ade56d812e7e214eeb66aeb9b8776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86218710"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Azure Kubernetes 服务 (AKS) 节点自动修复

AKS 会持续检查工作器节点的运行状况，在节点运行不正常的情况下对其进行自动修复。 本文档向操作员介绍了自动节点修复功能的运行方式。 除了 AKS 修复，Azure VM 平台还会对遇到问题的[虚拟机执行维护][vm-updates]。 AKS 和 Azure VM 协同工作，以最大程度地减少群集的服务中断次数。

> [!Important]
> Windows Server 节点池目前不支持节点自动修复功能。

## <a name="how-aks-checks-for-unhealthy-nodes"></a>AKS 如何检查运行不正常的节点

> [!Note]
> AKS 使用用户帐户 aks-remediator 对节点执行修复操作。

AKS 使用规则来确定节点是否运行不正常以及是否需要修复。 AKS 使用以下规则来确定是否需要自动修复。

* 节点在 10 分钟的时间范围内持续检查时报告状态为“未就绪”
* 节点在 10 分钟内未报告状态

可使用 kubectl 手动检查节点的运行状况状态。 

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>自动修复的工作原理

> [!Note]
> AKS 使用用户帐户“aks-remediator”启动修复操作。

默认情况下，VM 集类型为“虚拟机规模集”的群集支持自动修复。 如果根据上述规则确定节点为运行不正常，则 AKS 将在连续 10 分钟的不正常运行后重新启动该节点。 如果在初始修复操作后节点仍然运行不正常，则 AKS 工程师将调查其他修正。

如果在运行状况检查期间多个节点运行不正常，则在开始另一个修复之前，将单独修复每个节点。


<!--Not Avaiable on ## Next steps-->

<!--Not Avaiable on [Availability Zones][availability-zones]-->

<!-- LINKS - External -->

<!-- LINKS - Internal -->

<!--Not Avaiable on [availability-zones]: ./availability-zones.md-->

[vm-updates]: ../virtual-machines/maintenance-and-updates.md

<!-- Update_Description: update meta properties, wording update, update link -->