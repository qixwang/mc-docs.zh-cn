---
title: Azure 中的互操作性：测试设置 | Microsoft Docs
description: 本文介绍可用于分析 ExpressRoute、站点到站点 VPN 与 Azure 中虚拟网络对等互连之间的互操作性的测试设置。
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
origin.date: 10/18/2018
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: b7f04b5ea9e327f12b1f37fc55d2a81872c6cb88
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588414"
---
# <a name="interoperability-in-azure--test-setup"></a>Azure 中的互操作性：测试设置

本文介绍可用于分析 Azure 网络服务如何在控制平面级别和数据平面级别进行互操作的测试设置。 下面是 Azure 网络组件的概观：

-   **Azure ExpressRoute**：使用 Azure ExpressRoute 中的专用对等互连将本地网络中的专用 IP 空间直接连接到 Azure 虚拟网络部署。 ExpressRoute 有助于实现更高的带宽和专用连接。 许多 ExpressRoute 生态合作伙伴提供 ExpressRoute 连接并以 SLA 作为保障。 若要详细了解 ExpressRoute 及其配置方式，请参阅 [ExpressRoute 简介][ExpressRoute]。
-   **站点到站点 VPN**：可以使用 Azure VPN 网关作为站点到站点 VPN，通过 Internet 或 ExpressRoute 将本地网络安全连接到 Azure。 若要了解如何配置站点到站点 VPN 以连接到 Azure，请参阅[配置 VPN 网关][VPN]。
-   **VNet 对等互连**：使用虚拟网络 (VNet) 对等互连在 Azure 虚拟网络中的 VNet 之间建立连接。 若要详细了解 VNet 对等互连，请参阅[有关 VNet 对等互连的教程][VNet]。

## <a name="test-setup"></a>测试设置

下图演示了测试设置：

![1][1]

测试设置的中心部分是 Azure 区域 1 中的中心 VNet。 中心 VNet 按以下方式连接到不同的网络：

-   中心 VNet 使用 VNet 对等互连连接到辐射 VNet。 辐射 VNet 可远程访问中心 VNet 中的两个网关。
-   中心 VNet 使用站点到站点 VPN 连接到分支 VNet。 该连接使用 eBGP 来交换路由。
-   中心 VNet 使用 ExpressRoute 专用对等互连作为主要路径连接到本地位置 1 网络。 它使用站点到站点 VPN 连接作为备用路径。 在本文的余下部分，此 ExpressRoute 线路称作 ExpressRoute 1。 默认情况下，ExpressRoute 线路提供冗余连接以实现高可用性。 ExpressRoute 1 上已禁用面向辅助 Microsoft 企业边缘路由器 (MSEE) 的辅助客户边缘 (CE) 路由器子接口。 在上图中，双线箭头上面的红线表示禁用了 CE 路由器子接口。
-   中心 VNet 使用另一个 ExpressRoute 专用对等互连连接到本地位置 2 网络。 在本文的余下部分，这第二条 ExpressRoute 线路称作 ExpressRoute 2。
-   ExpressRoute 1 也将中心 VNet 和本地位置 1 网络连接到 Azure 区域 2 中的远程 VNet。

## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>串联 ExpressRoute 和站点到站点 VPN 连接

<!-- not support -->

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>将站点到站点 VPN 用作 ExpressRoute 的安全故障转移路径

ExpressRoute 充当冗余的线路对，可确保高可用性。 可在不同的 Azure 区域配置异地冗余的 ExpressRoute 连接。 另外，如测试设置中所示，在 Azure 区域中，可以使用站点到站点 VPN 为 ExpressRoute 连接创建故障转移路径。 通过 ExpressRoute 和站点到站点 VPN 播发相同的前缀时，Azure 会优先使用 ExpressRoute。 为了避免 ExpressRoute 与站点到站点 VPN 之间的非对称路由，本地网络配置同样应该优先使用 ExpressRoute 连接，然后再使用站点到站点 VPN 连接。

有关如何配置 ExpressRoute 和站点到站点 VPN 共存连接的详细信息，请参阅 [ExpressRoute 和站点到站点共存][ExR-S2S-CoEx]。

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>将后端连接扩展到辐射 VNet 和分支位置

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>使用 VNet 对等互连建立辐射 VNet 连接

中心辐射型 VNet 体系结构的使用非常广泛。 中心是 Azure 中的一个 VNet，充当辐射 VNet 与本地网络之间的连接中心点。 辐射是与中心对等互连的 VNet，可用于隔离工作负荷。 流量通过 ExpressRoute 或 VPN 连接在本地数据中心与中心之间流动。

<!-- For more information about the architecture, see [Implement a hub-spoke network topology in Azure][Hub-n-Spoke]. -->

在区域内的 VNet 对等互连中，辐射 VNet 可以使用中心 VPN 网关（VPN 和 ExpressRoute 网关）来与远程网络通信。

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>使用站点到站点 VPN 建立分支 VNet 连接

你可能想让位于不同区域中的分支 VNet 和本地网络通过中心 VNet 相互通信。 此配置的本机 Azure 解决方案是使用 VPN 建立站点到站点 VPN 连接。 替代方案是对中心内部的路由使用网络虚拟设备 (NVA)。

有关详细信息，请参阅[什么是 VPN 网关？][VPN]。

## <a name="next-steps"></a>后续步骤

了解测试拓扑的[配置详细信息][Configuration]。

了解测试设置的[控制平面分析][Control-Analysis]，以及拓扑中不同 VNet 或 VLAN 的视图。

了解测试设置的[数据平面分析][Data-Analysis]，以及 Azure 网络监视功能视图。

请参阅 [ExpressRoute 常见问题解答][ExR-FAQ]：
-   了解可将多少条 ExpressRoute 线路连接到一个 ExpressRoute 网关。
-   了解可将多少个 ExpressRoute 网关连接到一条 ExpressRoute 线路。
-   了解 ExpressRoute 的其他缩放限制。


<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "测试拓扑图"

<!--Link References-->
[ExpressRoute]: https://docs.azure.cn/expressroute/expressroute-introduction
[VPN]: https://docs.azure.cn/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.azure.cn/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: connectivty-interoperability-configuration.md
[Control-Analysis]: connectivty-interoperability-control-plane.md
[Data-Analysis]: connectivty-interoperability-data-plane.md
[ExR-FAQ]: https://docs.azure.cn/expressroute/expressroute-faqs

[ExR-S2S-CoEx]: https://docs.azure.cn/expressroute/expressroute-howto-coexist-resource-manager

<!-- [Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke -->
<!-- [Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha -->


