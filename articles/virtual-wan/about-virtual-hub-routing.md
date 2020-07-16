---
title: 关于虚拟中心路由
titleSuffix: Azure Virtual WAN
description: 本文介绍虚拟中心路由
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 06/29/2020
ms.date: 07/13/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 29aac40380937b83ac708caab29d0a9cd0dbedbb
ms.sourcegitcommit: 6c9e5b3292ade56d812e7e214eeb66aeb9b8776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86218848"
---
# <a name="about-virtual-hub-routing"></a>关于虚拟中心路由

虚拟中心的路由功能由路由器提供，该路由器使用边界网关协议 (BGP) 管理网关之间的所有路由。 虚拟中心可以包含多个网关，例如站点到站点 VPN 网关、ExpressRoute 网关、点到站点网关、Azure 防火墙。 该路由器还提供与虚拟中心连接的虚拟网络之间的传输连接，最高可支持 50 Gbps 的总吞吐量。 这些路由功能适用于标准虚拟 WAN 客户。

<!--Not Available on To configure routing, see [How to configure virtual hub routing](how-to-virtual-hub-routing.md)-->

<a name="concepts"></a>
## <a name="routing-concepts"></a>路由概念

以下部分介绍了虚拟中心路由的关键概念。

<a name="hub-route"></a>
### <a name="hub-route-table"></a>中心路由表

虚拟中心路由表可以包含一个或多个路由。 路由包括其名称、标签、目标类型、目标前缀列表和要路由的数据包的下一跃点信息。 “连接”通常具有一个与路由表关联或传播到路由表的路由配置

<a name="connection"></a>
### <a name="connection"></a>连接

连接是具有路由配置的资源管理器资源。 这四种连接类型为：

* **VPN 连接**：将 VPN 站点连接到虚拟中心 VPN 网关。
* **ExpressRoute 连接**：将 ExpressRoute 线路连接到虚拟中心 ExpressRoute 网关。
* **P2S 配置连接**：将用户 VPN（点到站点）配置连接到虚拟中心用户 VPN（点到站点）网关。
* **中心虚拟网络连接**：将虚拟网络连接到虚拟中心。

可以在安装期间为虚拟网络连接设置路由配置。 默认情况下，所有连接都关联并传播到默认路由表。

<a name="association"></a>
### <a name="association"></a>关联

每个连接都与一个路由表相关联。 将连接与路由表相关联，使你可以将流量发送到路由表中指定为路由的目标。 连接的路由配置将显示关联的路由表。  可以将多个连接关联到相同的路由表。 所有 VPN、ExpressRoute 和用户 VPN 连接都关联到同一（默认）路由表。

默认情况下，所有连接都与虚拟中心中的“默认路由表”相关联。 每个虚拟中心都有自己的默认路由表，可以对其进行编辑以添加静态路由。 如果前缀相同，静态添加的路由优先于动态获取的路由。

:::image type="content" source="./media/about-virtual-hub-routing/concepts-association.png" alt-text="关联":::

<a name="propagation"></a>
### <a name="propagation"></a>传播

连接将路由动态传播到路由表。 在 VPN 连接、ExpressRoute 连接或 P2S 配置连接中，使用 BGP 将路由从虚拟中心传播到本地路由器。 路由可以传播到一个或多个路由表。

可为每个虚拟中心使用“无路由表”。 传播到“无路由表”意味着不需要从连接传播路由。 VPN、ExpressRoute 和用户 VPN 连接将路由传播到同一组路由表。

:::image type="content" source="./media/about-virtual-hub-routing/concepts-propagation.png" alt-text="传播":::

<a name="static"></a>
### <a name="configuring-static-routes-in-a-virtual-network-connection"></a>在虚拟网络连接中配置静态路由

配置静态路由提供了一种机制来引导流量通过下一跃点 IP，这可以是与虚拟中心连接的分支 VNet 中预配的网络虚拟设备 (NVA) 的 IP。 静态路由由路由名称、目标前缀列表和下一跃点 IP 组成。

<a name="route"></a>
## <a name="route-tables-in-basic-and-standard-virtual-wans-prior-to-the-feature-set-of-association-and-propagation"></a>基本和标准虚拟 WAN 中的路由表优先于关联和传播功能集

路由表现在具有关联和传播功能。 预先存在的路由表是不具有这些功能的路由表。 如果中心路由中具有预先存在的路由，并希望使用新功能，请考虑以下事项：

* **在虚拟中心中具有预先存在的路由的标准虚拟 WAN 客户**：若要使用新的路由表功能，请从 Azure 门户中的中心的“路由”部分删除所有预先存在的路由，然后尝试在中心的“路由表”部分中创建新的路由表。

* **在虚拟中心中具有预先存在的路由的基本虚拟 WAN 客户**：若要使用新的路由表功能，请从 Azure 门户中的中心的“路由”部分删除所有预先存在的路由，然后将基本虚拟 WAN 升级到标准虚拟 WAN。 请参阅[将虚拟 WAN 从基本版升级到标准版](upgrade-virtual-wan.md)。

## <a name="next-steps"></a>后续步骤

<!--Not Available on [How to configure virtual hub routing](how-to-virtual-hub-routing.md)-->

有关虚拟 WAN 的详细信息，请参阅[常见问题解答](virtual-wan-faq.md)。

<!-- Update_Description: new article about about virtual hub routing -->
<!--NEW.date: 07/13/2020-->