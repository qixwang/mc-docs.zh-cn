---
title: include 文件
description: include 文件
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: include
origin.date: 11/22/2019
ms.date: 04/30/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 317a1f050375e24efd335623db47a42a98de78ca
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596206"
---
| 资源 | 限制 |
| --- | :--- |
| 每个订阅的最大群集数 | 100 |
| 包含虚拟机可用性集和基本负载均衡器 SKU 的每个群集的最大节点数  | 100 |
| 包含虚拟机规模集和[标准负载均衡器 SKU][standard-load-balancer] 的每个群集的最大节点数 | 1000（每个[节点池][node-pool] 100 个节点） |
| 每个节点的最大 Pod 数：带 Kubenet 的[基本网络][basic-networking] | 110 |
| 每个节点的最大 Pod 数：通过 Azure 容器联网界面进行[高级联网][advanced-networking] | Azure CLI 部署：30<sup>1</sup><br />Azure 资源管理器模板：30<sup>1</sup><br />门户部署：30 |

<!--Pending verify-->
<!--Not Available on Portal deployment: 30-->

<sup>1</sup>使用 Azure CLI 或资源管理器模板部署 Azure Kubernetes 服务 (AKS) 群集时，此值是可以配置的，最大可以配置为每节点 250 个 Pod。 在部署 AKS 群集以后，或者在使用 Azure 门户部署群集的情况下，不能配置每节点的最大 Pod 数。<br />

<!-- LINKS - Internal -->

[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking
[standard-load-balancer]: ../articles/load-balancer/load-balancer-standard-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->

[azure-support]: https://support.azure.cn/support/support-azure/

<!-- Update_Description: update meta properties, wording update, update link -->