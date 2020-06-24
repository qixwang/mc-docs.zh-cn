---
title: Azure Functions 网络选项
description: 在 Azure Functions 中可用的所有网络选项的概述。
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 06/08/2020
ms.author: v-junlch
ms.openlocfilehash: 28ea2ed9b00e8fa0dd9784f33721afa8727910a1
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84563601"
---
# <a name="azure-functions-networking-options"></a>Azure Functions 网络选项

本文介绍了可在各种 Azure Functions 托管选项中使用的网络功能。 通过以下所有网络选项，你可以在不使用可通过 Internet 路由的地址的情况下访问资源，或限制对函数应用的 Internet 访问。

托管模型具有不同的可用网络隔离级别。 选择正确模型有助于满足网络隔离要求。

可按多种方式托管函数应用：

* 可以从在多租户基础结构上运行的计划选项中进行选择，其中包含不同级别的虚拟网络连接和缩放选项：
    * [消耗计划](functions-scale.md#consumption-plan)会动态缩放以响应负载，并提供最少的网络隔离选项。
    * Azure [应用服务计划](functions-scale.md#app-service-plan)可按固定的规模运行。
* 可以在[应用服务环境](../app-service/environment/intro.md)中运行函数。 此方法可以将函数部署到虚拟网络中，并且可以进行完全的网络控制和隔离。

## <a name="matrix-of-networking-features"></a>网络功能矩阵

|                |[消耗计划](functions-scale.md#consumption-plan)|高级计划 |[应用服务计划](functions-scale.md#app-service-plan)|[应用服务环境](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[入站 IP 限制和专用站点访问](#inbound-ip-restrictions)|✅是|✅是|✅是|✅是|
|[虚拟网络集成](#virtual-network-integration)|❌否|✅是（区域）|✅是（区域和网关）|✅是|
|[虚拟网络触发器（非 HTTP）](#virtual-network-triggers-non-http)|❌否| ✅是 |✅是|✅是|
|[混合连接](#hybrid-connections)（仅限 Windows）|❌否|✅是|✅是|✅是|
|[出站 IP 限制](#outbound-ip-restrictions)|❌否| ✅是|✅是|✅是|

## <a name="inbound-ip-restrictions"></a>入站 IP 限制

可以使用 IP 限制来定义被允许或拒绝访问应用的 IP 地址的优先级排序列表。 此列表可以包含 IPv4 和 IPv6 地址。 如果存在一个或多个条目，则列表末尾会存在一个隐式的“拒绝所有”。 IP 限制可与所有函数托管选项配合使用。

> [!NOTE]
> 如果进行了网络限制，则只能从虚拟网络内部使用门户编辑器，或者在已将用于访问 Azure 门户的计算机的 IP 地址加入安全收件人列表之后使用该编辑器。 但是，仍可以从任何计算机访问“平台功能”选项卡中的任何功能。****

有关详细信息，请参阅 [Azure 应用服务静态访问限制](../app-service/app-service-ip-restrictions.md)。

## <a name="private-site-access"></a>专用站点访问

专用站点访问是指使应用只能从专用网络（例如 Azure 虚拟网络）进行访问。

* 配置了服务终结点时，[消耗](functions-scale.md#consumption-plan)和[应用服务](functions-scale.md#app-service-plan)计划中会提供专用站点访问。
    * 可以在“平台功能”**** > “网络”**** > “配置访问限制”**** > “添加规则”**** 下，针对每个应用配置服务终结点。 现在可以选择虚拟网络作为规则类型。
    * 有关详细信息，请参阅[虚拟网络服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)。
    * 请记住，使用服务终结点时，即使配置了虚拟网络集成，你的函数也还是对 Internet 具有完全出站访问权限。
* 已配置了内部负载均衡器 (ILB) 的应用服务环境内也提供专用站点访问。 有关详细信息，请参阅[在应用服务环境中创建并使用内部负载均衡器](../app-service/environment/create-ilb-ase.md)。

## <a name="virtual-network-integration"></a>虚拟网络集成

函数应用可以通过虚拟网络集成来访问虚拟网络内部的资源。
Azure Functions 支持两种类型的虚拟网络集成：

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

Azure Functions 中的虚拟网络集成使用与应用服务 Web 应用共享的基础结构。 若要详细了解这两种类型的虚拟网络集成，请参阅：

* [区域虚拟网络集成](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [需要网关的虚拟网络集成](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

## <a name="connect-to-service-endpoint-secured-resources"></a>连接到服务终结点保护的资源

为了提供更高级别的安全性，可以使用服务终结点将许多 Azure 服务限制在一个虚拟网络中。 然后，必须将函数应用与该虚拟网络集成，才能访问资源。 支持虚拟网络集成的所有计划都支持此配置。

若要了解详细信息，请参阅[虚拟网络服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)。

## <a name="restrict-your-storage-account-to-a-virtual-network"></a>将存储帐户限制到虚拟网络中

创建函数应用时，必须创建或链接到支持 Blob、队列和表存储的常规用途的 Azure 存储帐户。 目前不能对此帐户使用任何虚拟网络限制。 如果在用于函数应用的存储帐户上配置虚拟网络服务终结点，则该配置会中断应用。

有关详细信息，请参阅[存储帐户要求](./functions-create-function-app-portal.md#storage-account-requirements)。

## <a name="use-key-vault-references"></a>使用 Key Vault 引用

可以在不需进行任何代码更改的情况下，通过 Azure Key Vault 引用在 Azure Functions 应用程序中使用 Azure Key Vault 中的机密。 Azure Key Vault 是一项服务，可以提供集中式机密管理，并且可以完全控制访问策略和审核历史记录。

目前，如果密钥保管库使用服务终结点进行保护，则 [Key Vault 引用](../app-service/app-service-key-vault-references.md)不起作用。 若要使用虚拟网络集成连接到密钥保管库，需要在应用程序代码中调用 Key Vault。

## <a name="virtual-network-triggers-non-http"></a>虚拟网络触发器（非 HTTP）

目前，若要在虚拟网络中使用不同于 HTTP 的函数触发器，必须在应用服务计划或应用服务环境中运行函数应用。

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>具有虚拟网络触发器的应用服务计划和应用服务环境

函数应用在应用服务计划或应用服务环境中运行时，可以使用非 HTTP 触发器函数。 为了正确触发函数，必须连接到可访问在触发器连接中定义的资源的虚拟网络。

例如，假设要将 Azure Cosmos DB 配置为仅接受来自虚拟网络的流量。 在这种情况下，必须在应用服务计划中部署函数应用，该计划提供与该虚拟网络的虚拟网络集成。 集成使函数可以由该 Azure Cosmos DB 资源触发。

## <a name="hybrid-connections"></a>混合连接

[混合连接](../service-bus-relay/relay-hybrid-connections-protocol.md)是可用于访问其他网络中的应用程序资源的一项 Azure 中继功能。 使用混合连接可以从应用访问应用程序终结点。 不能使用混合连接来访问应用程序。 在除消耗计划之外的所有计划中，混合连接可用于 Windows 上运行的函数。

在 Azure Functions 中使用时，每个混合连接与单个 TCP 主机和端口组合相关联。 这意味着，混合连接终结点可以位于任何操作系统和任何应用程序上，前提是你能够访问 TCP 侦听端口。 混合连接功能不知道也不关心应用程序协议或者要访问的内容是什么。 它只提供网络访问。

有关详细信息，请参阅[应用服务文档中的“混合连接”](../app-service/app-service-hybrid-connections.md)。 这些相同的配置步骤支持 Azure Functions。

## <a name="outbound-ip-restrictions"></a>出站 IP 限制

出站 IP 限制在应用服务计划或应用服务环境中可用。 可以为部署了应用服务环境的虚拟网络配置出站限制。

将应用服务计划中的函数应用与虚拟网络集成时，默认情况下，该应用仍可对 Internet 进行出站调用。 通过添加应用程序设置 `WEBSITE_VNET_ROUTE_ALL=1`，可强制将所有出站流量发送到虚拟网络中，在其中可以使用网络安全组规则限制流量。

## <a name="troubleshooting"></a>故障排除

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="next-steps"></a>后续步骤

若要详细了解网络和 Azure Functions ，请参阅以下链接：

* [阅读 Azure Functions 网络常见问题解答](./functions-networking-faq.md)
* [详细了解虚拟网络与应用服务/Functions 的集成](../app-service/web-sites-integrate-with-vnet.md)
* [详细了解 Azure 中的虚拟网络](../virtual-network/virtual-networks-overview.md)
* [在应用服务环境中允许更多的网络功能和控制](../app-service/environment/intro.md)
* [在不更改防火墙的情况下使用混合连接连接到单个本地资源](../app-service/app-service-hybrid-connections.md)

<!-- Update_Description: wording update -->