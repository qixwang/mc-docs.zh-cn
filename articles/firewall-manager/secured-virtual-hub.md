---
title: 什么是安全虚拟中心？
description: 了解安全虚拟中心
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 8d2f4a31cd536d1ee64d8568be8940346d7efa72
ms.sourcegitcommit: 091c672fa448b556f4c2c3979e006102d423e9d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87162798"
---
# <a name="what-is-a-secured-virtual-hub"></a>什么是安全虚拟中心？

虚拟中心是 Microsoft 托管的虚拟网络，支持来自其他资源的连接。 在 Azure 门户中基于虚拟 WAN 创建虚拟中心时，还将创建一个虚拟中心 VNet 和网关（可选）作为其组件。

安全虚拟中心是一个 [Azure 虚拟 WAN 中心](../virtual-wan/virtual-wan-about.md#resources)，其中包含通过 Azure 防火墙管理器配置的关联的安全和路由策略。 使用安全虚拟中心，可通过本机安全服务轻松创建中心辐射型可传递体系结构，实现流量管理和保护。 

可以使用安全虚拟中心作为无本地连接的托管中心 VNet。 它取代了之前 Azure 防火墙部署所需的中心 VNet。 由于安全虚拟中心提供自动路由，因此无需配置自己的 UDR（用户定义的路由）即可通过防火墙路由流量。

还可以将安全虚拟中心用作完整虚拟 WAN 体系结构的一部分。 此体系结构提供到 Azure 以及通过 Azure 的安全、经过优化的自动分支连接。 你可以选择用于保护和管理网络流量的服务，包括 Azure 防火墙和其他第三方安全即服务 (SECaaS) 提供程序。

## <a name="create-a-secured-virtual-hub"></a>创建安全虚拟中心

使用 Azure 门户中的防火墙管理器，可以创建新的安全虚拟中心，也可使用 Azure 虚拟 WAN 转换之前创建的现有虚拟中心。

## <a name="next-steps"></a>后续步骤

要创建安全虚拟中心并使用它来保护和管理中心和分支网络，请参阅[教程：在 Azure 门户中使用 Azure 防火墙管理器保护云网络](secure-cloud-network.md)。