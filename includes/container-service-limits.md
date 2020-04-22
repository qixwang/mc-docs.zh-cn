---
title: include 文件
description: include 文件
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: include
origin.date: 10/11/2018
ms.date: 05/13/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 89b3478e242f7682af244f23f023f1cc399d1bfc
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "65520688"
---
| 资源 | 默认限制 |
| --- | :--- |
| 每个订阅的最大群集数 | 100 |
| 每个群集的最大节点数 | 100 |
| 每个节点的最大 Pod 数：带 Kubenet 的[基本网络][basic-networking] | 110 |
| 每个节点的最大 Pod 数：通过 Azure 容器联网界面进行[高级联网][advanced-networking] | Azure CLI 部署：30<sup>1</sup><br />Azure 资源管理器模板：30<sup>1</sup><br />门户部署：30 |

<!--Pending verify-->
<!--Not Available on Portal deployment: 30-->

<sup>1</sup>使用 Azure CLI 或资源管理器模板部署 Azure Kubernetes 服务 (AKS) 群集时，此值是可以配置的，最大可以配置为每节点 250 个 Pod。 在部署 AKS 群集以后，或者在使用 Azure 门户部署群集的情况下，不能配置每节点的最大 Pod 数。<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://support.azure.cn/zh-cn/support/support-azure/