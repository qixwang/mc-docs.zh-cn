---
title: 采用 Azure ExpressRoute 所要满足的先决条件 | Azure
description: 本页提供了在订购 Azure ExpressRoute 线路之前需要满足的要求列表。 其中包括了一个清单。
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: ''
ms.assetid: f872d25e-acfd-405d-9d1b-dcb9f323a2ff
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 09/18/2019
ms.date: ''
ms.author: v-yiso
ms.openlocfilehash: 170ba9112f593b5515db72b404da48836c660ea0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914369"
---
# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute 先决条件和清单  

若要使用 ExpressRoute 连接到 Microsoft 云服务，需确认是否符合以下部分中所列的要求。

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure 帐户
* 使用中的有效 Microsoft Azure 帐户。 需有此帐户才能设置 ExpressRoute 线路。 ExpressRoute 线路是 Azure 订阅中的资源。 即使连接限于非 Azure Microsoft 云服务（例如 Office 365 服务和 Dynamics 365），Azure 订阅也是一个必要条件。
* 有效的 Office 365 订阅（如果使用的是 Office 365 服务）。 

## <a name="connectivity-provider"></a>连接服务提供商
- 可使用 [ExpressRoute 连接合作伙伴](./expressroute-locations.md#partners) 连接到 Microsoft 云。 可以通过[三种方法](./expressroute-introduction.md)在本地网络与 Microsoft 之间设置连接。 
- 即使提供商不是 ExpressRoute 连接合作伙伴，也可通过[云交换提供商](./expressroute-locations.md#nonpartners)连接到 Microsoft 云。

## <a name="network-requirements"></a>网络要求
* **每个对等互连位置的冗余性**：Microsoft 要求在 Microsoft 的路由器和每个 ExpressRoute 线路上的对等互连路由器之间建立冗余的 BGP 会话（即使只有[一个到云交换的物理连接](expressroute-faqs.md#onep2plink)）。
* **灾难恢复的冗余**：Microsoft 强烈建议你在不同的对等互连位置设置至少两条 ExpressRoute 线路，避免单点故障。
* **路由**：你或提供商需设置和管理针对[路由域](expressroute-circuit-peerings.md)的 BGP 会话，具体取决于连接到 Microsoft 云的方式。 某些以太网连接服务提供商或云交换服务提供商可能会以增值服务的形式提供 BGP 管理。
* **NAT**：Microsoft 只能通过 Microsoft 对等互连接受公共 IP 地址。 如果使用的是本地网络中的专用 IP 地址，你或提供商需[使用 NAT](expressroute-nat.md)将专用 IP 地址转换为公共 IP 地址。



## <a name="next-steps"></a>后续步骤

- 有关 ExpressRoute 的详细信息，请参阅 [ExpressRoute 常见问题](./expressroute-faqs.md)。
- 查找 ExpressRoute 连接服务提供商。 请参阅 [ExpressRoute 合作伙伴和对等位置](./expressroute-locations.md)。
- 请参阅[路由](expressroute-routing.md)、[NAT](expressroute-nat.md) 的要求。
- 配置 ExpressRoute 连接。
  - [创建 ExpressRoute 线路](expressroute-howto-circuit-arm.md)
  - [配置路由](expressroute-howto-routing-arm.md)
  - [将 VNet 链接到 ExpressRoute 线路](expressroute-howto-linkvnet-arm.md)
    
<!--Update_Description:update meta properties only-->    