---
title: 体系结构：全球传输网络体系结构
titleSuffix: Azure Virtual WAN
description: 了解虚拟 WAN 的全局传输网络体系结构
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: article
origin.date: 02/06/2020
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: 3f985067a1d338a0deddc69f857bc3eab48195f2
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155023"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>全局传输网络体系结构和虚拟 WAN

现代企业需要在云中和本地的超分布式应用程序、数据与用户之间随时建立连接。 有许多企业正在采用全局传输网络体系结构来整合、连接和控制以云为中心的现代全球企业 IT 设施。

全局传输网络体系结构基于经典的中心辐射型连接模型，使用其中的云托管网络“中心”可以在跨不同“辐射”类型分布的终结点之间建立传输连接。

在此模型中，辐射可以是：
* 虚拟网络 (VNet)
* 物理分支站点
* 远程用户
* Internet

![中心和辐射](./media/virtual-wan-global-transit-network-architecture/figure1.png)

图 1：**全局传输中心辐射型网络**

图 1 显示了全局传输网络的逻辑视图，其中的地理分散式用户、物理站点和 VNet 通过云中托管的网络中心互连。 此体系结构可在网络终结点之间实现逻辑单跃点传输连接。

<a name="globalnetworktransit"></a>
## <a name="global-transit-network-with-virtual-wan"></a>使用虚拟 WAN 的全局传输网络

Azure 虚拟 WAN 是 Azure 托管的云网络服务。 构成此服务的所有网络组件由 Azure 托管和管理。 有关虚拟 WAN 的详细信息，请参阅[虚拟 WAN 概述](virtual-wan-about.md)一文。

Azure 虚拟 WAN 支持在 VNet 中的全局分布式云工作负荷集、分支站点、SaaS 和 PaaS 应用程序与用户之间随时建立任意点到任意点的连接，从而实现全局传输网络体系结构。

![Azure 虚拟 WAN](./media/virtual-wan-global-transit-network-architecture/figure2.png)

图 2：**全局传输网络和虚拟 WAN**

在 Azure 虚拟 WAN 体系结构中，虚拟 WAN 中心在可将分支、VNet 和远程用户连接到的 Azure 区域中预配。 物理分支站点通过高级 ExpressRoute 或站点到站点 VPN 连接到中心，VNet 通过 VNet 连接连接到中心，远程用户可以使用用户 VPN（点到站点 VPN）直接连接到中心。 虚拟 WAN 还支持跨区域 VNet 连接，其中，一个区域中的 VNet 可以连接到另一个区域中的虚拟 WAN 中心。

若要建立虚拟 WAN，可以在包含最多辐射（分支、VNet、用户）的区域中创建单个虚拟 WAN 中心，然后将其他区域中的辐射连接到该中心。 如果企业的运营足迹主要在包括少量几个远程辐射的单个区域内，则这是一个不错的选择。  

<a name="hubtohub"></a>
## <a name="hub-to-hub-connectivity"></a>中心到中心的连接

企业云足迹可以跨多个云区域，最好是从最靠近其物理站点和用户的区域访问云（改善延迟）。 全局传输网络体系结构的重要原则之一是在所有云与本地网络终结点之间实现跨区域连接。 这意味着，与一个区域中的云相连接的分支发出的流量，可以使用 [Azure 全球网络](https://www.azure.cn/global-infrastructure/global-network/)实现的中心到中心连接抵达不同区域中的另一个分支或 VNet。

![跨区域](./media/virtual-wan-global-transit-network-architecture/figure3.png)

图 3：**虚拟 WAN 跨区域连接**

在单个虚拟 WAN 中启用多个中心时，中心将通过中心到中心的链接自动互连，从而在跨多个区域分布的分支与 VNet 之间实现全局连接。 

此外，全部属于同一虚拟 WAN 的中心可与不同的区域访问和安全策略相关联。 有关详细信息，请参阅本文稍后的[安全和策略控制](#security)。

<a name="anytoany"></a>
## <a name="any-to-any-connectivity"></a>任意点到任意点的连接

全局传输网络体系结构通过虚拟 WAN 中心实现任意点到任意点的连接。 此体系结构消除或减少了辐射之间的全网格或部分网格式连接模型的需要，此类模型的构建和维护更复杂。 此外，与网格网络相比，中心辐射型体系结构中的路由控制更容易配置和维护。

在全局体系结构环境中，任意点到任意点的连接可让企业中的全局分布式用户、分支、数据中心、VNet 和应用程序通过传输中心相互连接。 Azure 虚拟 WAN 充当全局传输系统。

![任意点到任意点](./media/virtual-wan-global-transit-network-architecture/figure4.png)

图 4：**虚拟 WAN 流量路径**

Azure 虚拟 WAN 支持以下全局传输连接路径。 括号中的字母对应于图 4 中的标识。

* 分支到 VNet (a)
* 分支到分支 (b)
    * 虚拟 WAN
    
    <!--Not Avaialble on ExpressRoute Global Reach-->
    
* 远程用户到 VNet (c)
* 远程用户到分支 (d)
* VNet 到 VNet (e)
* 分支到中心/中心到分支 (f)
* 分支到中心/中心到 VNet (g)
* VNet 到中心/中心到 VNet (h)

### <a name="branch-to-vnet-a-and-branch-to-vnet-cross-region-g"></a>分支到 VNet (a)/分支到 VNet 跨区域 (g)

分支到 VNet 是 Azure 虚拟 WAN 支持的主要路径。 使用此路径可将分支连接到 Azure VNet 中部署的 Azure IAAS 企业工作负荷。 分支可以通过 ExpressRoute 或站点到站点 VPN 连接到虚拟 WAN。 流量通过 VNet 连接传输到与虚拟 WAN 中心相连接的 VNet。 虚拟 WAN 不需要明确的[网关传输](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity)，因为虚拟 WAN 会自动启用到分支站点的网关传输。 请参阅[虚拟 WAN 合作伙伴](virtual-wan-configure-automation-providers.md)一文，了解如何将 SD-WAN CPE 连接到虚拟 WAN。

### <a name="virtual-wan"></a>虚拟 WAN

<!--Not Avaialble on ExpressRoute Global Reach-->

<!--Not Avaialble on ExpressRoute is a private and resilient way to connect your on-premises networks to the Azure Cloud. Virtual WAN supports Express Route circuit connections. Connecting a branch site to Virtual WAN with Express Route requires 1) Premium Circuit 2) Circuit to be in a Global Reach enabled location.-->

<!--Not Avaialble on ExpressRoute Global Reach is an add-on feature for ExpressRoute. With Global Reach, you can link ExpressRoute circuits together to make a private network between your on-premises networks. Branches that are connected to Azure Virtual WAN using ExpressRoute require the ExpressRoute Global Reach to communicate with each other.-->

在此模型中，使用 ExpressRoute 连接到虚拟 WAN 中心的每个分支可以使用分支到 VNet 的路径连接到 VNet。

<!--Not Avaialble on  Branch-to-branch traffic won't transit the hub because ExpressRoute Global Reach enables a more optimal path over Azure WAN.-->

### <a name="branch-to-branch-b-and-branch-to-branch-cross-region-f"></a>分支到分支 (b)/分支到分支跨区域 (f)

分支可以使用 ExpressRoute 线路和/或站点到站点 VPN 连接连接到 Azure 虚拟 WAN 中心。 可将分支连接到其最靠近的区域中的虚拟 WAN 中心。

企业可以通过此选项来利用 Azure 主干网连接分支。 但是，尽管可以此功能，应该权衡通过 Azure 虚拟 WAN 与通过专用 WAN 连接分支的利弊。  

### <a name="remote-user-to-vnet-c"></a>远程用户到 VNet (c)

使用远程用户客户端与虚拟 WAN 之间的点到站点连接可以安全地直接远程访问 Azure。 企业远程用户不再需要使用企业 VPN 来始终与云保持连接。

### <a name="remote-user-to-branch-d"></a>远程用户到分支 (d)

使用远程用户到分支路径，与 Azure 建立了点到站点连接的远程用户可以通过云传输流量，以此访问本地工作负荷和应用程序。 此路径可让远程用户灵活访问部署在 Azure 中和本地的工作负荷。 企业可以在 Azure 虚拟 WAN 中启用基于中心云的安全远程访问服务。

### <a name="vnet-to-vnet-transit-e-and-vnet-to-vnet-cross-region-h"></a>VNet 到 VNet 传输 (e)/VNet 到 VNet 跨区域 (h)

VNet 到 VNet 传输使 VNet 能够相互连接，以互连跨多个 VNet 实施的多层应用程序。 或者，可以通过 VNet 对等互连将 VNet 相互连接，这可能适合无需通过 VWAN 中心进行传输的某些方案。

<a name="security"></a>
## <a name="security-and-policy-control"></a>安全和策略控制

Azure 虚拟 WAN 中心将互连整个混合网络中的所有网络终结点，可能会看到所有传输网络流量。 可以在 VWAN 中心内部署 Azure 防火墙以启用基于云的安全、访问和策略控制，将虚拟 WAN 中心转换为安全虚拟中心。 虚拟 WAN 中心内的 Azure 防火墙协调可由 Azure 防火墙管理器执行。

[Azure 防火墙管理器](https://go.microsoft.com/fwlink/?linkid=2107683)提供管理和缩放全局传输网络安全性的功能。 Azure 防火墙管理器提供通过第三方工具和 Azure 防火墙集中管理路由、全局策略和高级 Internet 安全服务的功能。

![使用 Azure 防火墙的安全虚拟中心](./media/virtual-wan-global-transit-network-architecture/figure5.png)

图 5：**使用 Azure 防火墙的安全虚拟中心**

虚拟 WAN 的 Azure 防火墙支持以下全局安全传输连接路径。 括号中的字母对应于图 5 中的标识。

* VNet 到 VNet 安全传输 (e)
* VNet 到 Internet 或第三方安全服务 (i)
* 分支到 Internet 或第三方安全服务 (j)

### <a name="vnet-to-vnet-secured-transit-e"></a>VNet 到 VNet 安全传输 (e)

VNet 到 VNet 安全传输使 VNet 能够通过虚拟 WAN 中心内的 Azure 防火墙相互连接。

### <a name="vnet-to-internet-or-third-party-security-service-i"></a>VNet 到 Internet 或第三方安全服务 (i)

VNet 到 Internet 或第三方安全传输使 VNet 能够通过虚拟 WAN 中心内的 Azure 防火墙连接到 Internet 或支持的第三方安全服务。

### <a name="branch-to-internet-or-third-party-security-service-j"></a>分支到 Internet 或第三方安全服务 (j)
分支到 Internet 或第三方安全传输使分支能够通过虚拟 WAN 中心内的 Azure 防火墙连接到 Internet 或支持的第三方安全服务。

## <a name="next-steps"></a>后续步骤

使用虚拟 WAN 创建连接并在 VWAN 中心内部署 Azure 防火墙。

* [使用虚拟 WAN 的站点到站点连接](virtual-wan-site-to-site-portal.md)

<!--Not Available on * [Point-to-site connections using Virtual WAN](virtual-wan-point-to-site-portal.md)-->
<!--Not Available on * [ExpressRoute connections using Virtual WAN](virtual-wan-expressroute-portal.md)-->

<!-- Update_Description: update meta properties, wording update, update link -->