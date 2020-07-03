---
title: 创建 Azure Kubernetes 服务群集资源
titleSuffix: Azure Cognitive Services
description: 了解如何创建 Azure Kubernetes 服务 (AKS) 资源。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2020
origin.date: 04/01/2020
ms.author: v-tawe
ms.openlocfilehash: 242888308a472b2aa9bea1d1d9d1817848ce55e7
ms.sourcegitcommit: f5484e21fa7c95305af535d5a9722b5ab416683f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85323247"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>创建 Azure Kubernetes 服务群集资源

1. 转到 [Azure Kubernetes 服务](https://portal.azure.cn/#create/microsoft.aks)，然后选择“创建”。

1. 在“基本信息”选项卡中输入以下信息：

    |设置|Value|
    |--|--|
    |订阅|选择相应的订阅。|
    |资源组|选择可用的资源组。|
    |Kubernetes 群集名称|输入名称（小写）。|
    |区域|选择附近的位置。|
    |Kubernetes 版本|标记为“(默认值)”的任何值。|
    |DNS 名称前缀|自动创建，但可以重写。|
    |节点大小|标准 DS2 v2：<br>`2 vCPUs`, `7 GB`|
    |节点计数|将滑块留在默认值的位置。|

1. 在“缩放”选项卡上，将“虚拟节点”和“VM 规模集”设置为其默认值。
1. 在“身份验证”选项卡上，将“服务主体”和“启用 RBAC”设为其默认值。  
1. 在“网络”选项卡上，输入以下选择：

    |设置|Value|
    |--|--|
    |网络配置|基本|

1. 在“监视”选项卡上，确保将“启用容器监视”设置为“是”，并让“Log Analytics 工作区”保留默认值   。
1. 在“标记”选项卡上，暂时让名称/值对保留空白。
1. 选择“查看并创建”。
1. 通过验证后，选择“创建”。

> [!NOTE]
> 如果验证失败，可能是由于“服务主体”错误。 返回到“身份验证”选项卡，然后回到“查看 + 创建”，应该会在其中运行验证，然后验证会通过。 
