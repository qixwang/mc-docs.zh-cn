---
title: 将应用与 Azure 虚拟网络进行集成
description: 将 Azure 应用服务中的应用与 Azure 虚拟网络集成。
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
origin.date: 02/27/2020
ms.date: 03/16/2020
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: 11d54a67e700aa02c68035455e3b6d9eab456ad0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79546921"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>将应用与 Azure 虚拟网络进行集成
本文档介绍 Azure 应用服务虚拟网络集成功能，并说明如何在 [Azure 应用服务](overview.md)中使用应用对其进行设置。 使用 [Azure 虚拟网络][VNETOverview] (VNet) 可将多个 Azure 资源置于无法通过 Internet 路由的网络中。

Azure 应用服务有两种变体。

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

## <a name="enable-vnet-integration"></a>启用 VNet 集成 

1. 在应用服务门户中转到“网络”UI。 在“VNet 集成”下，选择“单击此处进行配置”。  

1. 选择“添加 VNet”  。  

   ![选择 VNet 集成][1]

1. 下拉列表将包含所有其他区域中的所有资源管理器 VNet。 选择要与之集成的 VNet。

   ![选择 VNet][2]

   <!-- * If the VNet is in the same region, then either create a new subnet or pick an empty pre-existing subnet.  -->

   * 若要选择另一区域中的 VNet，必须已预配一个启用了点到站点连接的虚拟网络网关。

   * 若要与经典 VNet 集成，请不要单击“VNet”下拉列表，而应选择“单击此处连接到经典 VNet”。  选择所需的经典 VNet。 目标 VNet 中必须已预配一个启用了点到站点连接的虚拟网络网关。

    ![选择经典 VNet][3]
    
在集成期间，应用会重启。  完成集成后，将会看到有关集成的 VNet 的详细信息。 

将应用与 VNet 集成后，该应用将使用配置给 VNet 的同一个 DNS 服务器，除非它是 Azure DNS 专用区域。 目前无法对 Azure DNS 专用区域使用 VNet 集成。

<!-- ## Regional VNet Integration -->
<!-- Add VNet (preview) not available in mc portal-->

## <a name="gateway-required-vnet-integration"></a>需要网关的 VNet 集成

需要网关的 VNet 集成支持连接到另一区域中的 VNet，或连接到经典 VNet。 需要网关的 VNet 集成： 

* 允许应用一次只连接到 1 个 VNet
* 允许在一个应用服务计划中最多集成 5 个 VNet 
* 允许在应用服务计划中由多个应用使用同一个 VNet，不影响可供应用服务计划使用的总数。  如果有 6 个应用在使用同一应用服务计划中的同一 VNet，则算作是使用了 1 个 VNet。 
* 由于网关上的 SLA，可实现 99.9% 的 SLA
* 允许应用使用配置给 VNet 的 DNS
* 需要在基于虚拟网络路由的网关中配置 SSTP 点到站点 VPN，才能将此网关连接到应用。 

需要网关的 VNet 集成不可用于：

<!-- * With Linux apps -->

* 通过 ExpressRoute 连接的 VNet 
* 访问服务终结点保护的资源
* 既支持 ExpressRoute，也支持点到站点/站点到站点 VPN 的共存网关

### <a name="set-up-a-gateway-in-your-vnet"></a>在 VNet 中设置网关 ###

若要创建网关，请执行以下操作：

1. 在 VNet 中[创建网关子网][creategatewaysubnet]。  

1. [创建 VPN 网关][creategateway]。 选择基于路由的 VPN 类型。

1. [设置点到站点地址][setp2saddresses]。 如果网关不在基本 SKU 中，则必须在点到站点配置中禁用 IKEV2 并选择 SSTP。 点到站点地址空间必须在 RFC 1918 地址块 10.0.0.0/8、172.16.0.0/12、192.168.0.0/16 中

如果只是创建网关用于应用服务 VNet 集成，则不需要上传证书。 创建网关可能需要 30 分钟。 若要将应用与 VNet 集成，必须先预配网关。 

### <a name="how-gateway-required-vnet-integration-works"></a>需要网关的 VNet 集成的工作原理

需要网关的 VNet 集成基于点到站点 VPN 技术。 点到站点 VPN 将网络访问限制于托管应用的虚拟机。 应用受到限制，只能通过混合连接或 VNet 集成向外发送流量至 Internet。 通过门户将应用配置为使用需要网关的 VNet 集成后，系统会代你管理复杂的协商，以便在网关上和应用程序端创建并分配证书。 最终的结果是，用于托管应用的辅助角色能够直接连接到所选 VNet 中的虚拟网络网关。 

![需要网关的 VNet 集成的工作原理][6]

### <a name="accessing-on-premises-resources"></a>访问本地资源

应用可以通过与具备站点到站点连接的 VNet 集成来访问本地资源。 如果使用网关所需的 VNet 集成，需要使用点到站点地址块更新本地 VPN 网关路由。 先设置站点到站点 VPN，接着应通过用于配置该 VPN 的脚本来正确地设置路由。 如果在创建站点到站点地址后才添加点到站点 VPN，则需手动更新路由。 具体操作信息取决于每个网关，在此不作说明。 不能使用站点到站点 VPN 连接配置 BGP。

<!-- There is no additional configuration required for the regional VNet Integration feature to reach through your VNet, and  -->

本地连接。 只需使用 ExpressRoute 或站点到站点 VPN 将 VNet 连接到本地。 

> [!NOTE]
> 需要网关的 VNet 集成功能不会将应用与包含 ExpressRoute 网关的 VNet 集成。 即使以[共存模式][VPNERCoex]配置 ExpressRoute 网关，VNet 集成也不会生效。
> 

<!--  If you need to access resources through an ExpressRoute connection, then you can use the regional VNet Integration feature or an [App Service Environment][ASE], which runs in your VNet.  -->

### <a name="peering"></a>对等互连

<!-- If you are using peering with the regional VNet Integration, you do not need to do any additional configuration.  -->

如果结合对等互连使用网关所需的 VNet 集成，则需要配置几个附加的项。 若要配置对等互连以使用应用，请执行以下操作：

1. 在应用所连接的 VNet 上添加对等互连连接。 在添加对等互连连接时，启用“允许虚拟网络访问”并单击“允许转发流量”和“允许网关传输”    。
1. 在与所连接的 VNet 对等互连的 VNet 上添加对等互连连接。 在目标 VNet 上添加对等互连连接时，启用“允许虚拟网络访问”并单击“允许转发流量”和“允许远程网关”    。
1. 转到门户中的“应用服务计划”>“网络”>“VNet 集成 UI”。  选择应用连接的 VNet。 在路由部分下，添加与应用所连接的 VNet 对等互连的 VNet 的地址范围。  

## <a name="managing-vnet-integration"></a>管理 VNet 集成 

与 VNet 连接和断开连接是在应用级别发生的。 可能影响多个应用的 VNet 集成的操作在应用服务计划级别执行。 可以通过“应用 > 网络 > VNet 集成门户”获取 VNet 的详细信息。 可以在“ASP”>“网络”>“VNet 集成门户”中查看 ASP 级别的类似信息。

在 VNet 集成的应用视图中，能够执行的唯一操作是断开应用与当前所连接到的 VNet 的连接。 若要断开应用与 VNet 的连接，请选择“断开连接”  。 在从 VNet 断开连接时，应用将会重启。 断开连接操作不会更改 VNet。 不会删除子网或网关。 若要删除 VNet，需要先从该 VNet 断开连接应用，然后删除该 VNet 中的资源，例如网关。 

ASP VNet 集成 UI 会显示 ASP 中的应用使用的所有 VNet 集成。 要查看每个 VNet 的详细信息，请单击感兴趣的 VNet。 在此处，可以针对需要网关的 VNet 集成执行两项操作。

* **同步网络**。 同步网络操作仅适用于网关相关的 VNet 集成功能。 执行同步网络操作可确保证书和网络信息保持同步。如果添加或更改 VNet 的 DNS，则需执行“同步网络”操作  。 此操作会重启所有使用此 VNet 的应用。
* **添加路由** 添加路由会驱动出站流量进入 VNet。 

**需要网关的 VNet 集成路由** 在 VNet 中定义的路由，用于将流量从应用导入 VNet。 如果需要将其他出站流量发送到 VNet 中，则可以在此处添加地址块。 此功能只适用于网关所需的 VNet 集成。 使用需要网关的 VNet 集成时，路由表不会影响应用流量。

**需要网关的 VNet 集成证书** 启用需要网关的 VNet 集成后，必须进行证书交换以确保连接的安全性。 除了证书，还有 DNS 配置、路由以及其他类似的用于描述网络的内容。
如果更改了证书或网络信息，则需单击“同步网络”。 单击“同步网络”会导致应用与 VNet 之间的连接出现短暂的中断。 虽然应用不会重启，但失去连接会导致站点功能失常。 

## <a name="pricing-details"></a>定价详细信息

<!-- The regional VNet Integration feature has no additional charge for use beyond the ASP pricing tier charges. -->

使用网关所需的 VNet 集成功能涉及到三种相关费用：

* ASP 定价层费用 - 应用需要属于“标准”、“高级”或“高级 V2”应用服务计划。 可在此处了解这些费用的更多详细信息：[应用服务定价][ASPricing]。 
* 数据传输费用 - 数据传出会产生费用，即使 VNet 在同一数据中心也是如此。 [数据传输定价详细信息][DataPricing]中对这些费用进行了说明。 
* VPN 网关费用 - 点到站点 VPN 所需的 VPN 网关会产生费用。 [VPN 网关定价][VNETPricing]页上介绍了详细信息。

## <a name="troubleshooting"></a>故障排除

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="automation"></a>自动化

<!-- regional VNet Integration
There is CLI support for regional VNet Integration. To access to the following commands, [Install Azure CLI][installCLI]. 

        az webapp vnet-integration --help

        Group
            az webapp vnet-integration : Methods that list, add, and remove virtual network integrations
            from a webapp.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            add    : Add a regional virtual network integration to a webapp.
            list   : List the virtual network integrations on a webapp.
            remove : Remove a regional virtual network integration from webapp.

        az appservice vnet-integration --help

        Group
            az appservice vnet-integration : A method that lists the virtual network integrations used in an
            appservice plan.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            list : List the virtual network integrations used in an appservice plan.
-->

对于需要网关的 VNet 集成，可以使用 PowerShell 将应用服务与 Azure 虚拟网络相集成。 有关就绪可运行的脚本，请参阅 [Connect an app in Azure App Service to an Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3)（将 Azure 应用服务中的应用连接到 Azure 虚拟网络）。


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-classic.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-regionalworks.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-gwworks.png


<!--Links-->
[VNETOverview]: /virtual-network/virtual-networks-overview
[AzurePortal]: https://portal.azure.cn/
[ASPricing]: https://www.azure.cn/pricing/details/app-service/
[VNETPricing]: https://www.azure.cn/pricing/details/vpn-gateway/
[DataPricing]: https://www.azure.cn/pricing/details/data-transfer/
[V2VNETP2S]: https://www.azure.cn/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[creategateway]: https://docs.azure.cn/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.azure.cn/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
[VNETRouteTables]: https://docs.azure.cn/virtual-network/manage-route-table/
[installCLI]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest/
