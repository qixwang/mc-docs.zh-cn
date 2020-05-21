---
title: Azure Stack Hub 上的事件中心概述
description: 了解 Azure Stack Hub 上的事件中心资源提供程序。
author: WenJason
ms.author: v-jay
ms.service: azure-stack
ms.topic: how-to
origin.date: 12/09/2019
ms.date: 05/18/2020
ms.reviewer: jfggdl
ms.lastreviewed: 12/09/2019
ms.openlocfilehash: f2e0e905c9dbf9079b36dbda9e9669d10192533d
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83423034"
---
# <a name="event-hubs-on-azure-stack-hub-overview"></a>Azure Stack Hub 上的事件中心概述

[!INCLUDE [preview-banner](../includes/event-hubs-preview.md)]

使用 Azure Stack Hub 上的事件中心可以实现混合云方案。 支持使用基于流式处理和事件的解决方案进行本地处理和 Azure 云处理。 无论方案是混合（联网）还是离线的，解决方案都支持大规模的事件/流处理。 方案仅受群集大小的约束，但你可以根据需要预配群集大小。 

## <a name="features"></a>功能

请参阅 [Azure Stack Hub 用户文档](/azure-stack/user/event-hubs-overview)，以便在 Azure Stack 上的事件中心与 Azure 事件中心之间进行功能比较。

## <a name="feature-documentation"></a>功能文档

若要详细了解事件中心用户体验，请参阅 [Azure 事件中心文档](/event-hubs/)。 此文档适用于两个版本的事件中心，并且包含以下主题：

- 有关[事件中心概念](/event-hubs/event-hubs-features)的详细信息
- 如何[创建事件中心群集和命名空间](/event-hubs/event-hubs-dedicated-cluster-create-portal)
- 如何创建[事件中心](/event-hubs/event-hubs-create#create-an-event-hub)
- 如何[使用 Kafka 协议](/event-hubs/event-hubs-quickstart-kafka-enabled-event-hubs)进行流式传输


## <a name="next-steps"></a>后续步骤

在开始安装过程之前，请查看 [Azure Stack Hub 上事件中心的容量规划](event-hubs-rp-capacity-planning.md)。 了解容量规划有助于确保用户拥有所需的容量。