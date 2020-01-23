---
title: Azure Functions 网络选项
description: 在 Azure Functions 中可用的所有网络选项的概述。
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: v-junlch
ms.openlocfilehash: 729c0a32492e541161d799eb5085235698d6c331
ms.sourcegitcommit: 48d51745ca18de7fa05b77501b4a9bf16cea2068
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76116862"
---
# <a name="azure-functions-networking-options"></a>Azure Functions 网络选项

本文介绍了可在各种 Azure Functions 托管选项中使用的网络功能。 以下所有网络选项都可以为你提供某种功能，用来在不使用可通过 Internet 路由的地址的情况下访问资源，或限制对函数应用的 Internet 访问。

托管模型具有不同的可用网络隔离级别。 选择适当的级别有助于满足网络隔离要求。

可按多种方式托管函数应用：

* 有一组计划选项可在多租户基础结构上运行，并提供不同级别的虚拟网络连接和缩放选项：
    * [消耗计划](functions-scale.md#consumption-plan)：可以动态缩放以响应负载，但只能提供极少量的网络隔离选项。
    * Azure [应用服务计划](functions-scale.md#app-service-plan)：可按固定的规模运行。
* 可以在[应用服务环境](../app-service/environment/intro.md)中运行函数。 此方法可以将函数部署到虚拟网络中，并且可以进行完全的网络控制和隔离。

## <a name="matrix-of-networking-features"></a>网络功能矩阵

|                |[消耗计划](functions-scale.md#consumption-plan) |[应用服务计划](functions-scale.md#app-service-plan)| 
|----------------|-----------|----------------|---------|-----------------------|  
|[入站 IP 限制和专用站点访问](#inbound-ip-restrictions)|✅是 |✅是|
|[虚拟网络集成](#virtual-network-integration)|❌否 |✅是（区域和网关）| 
|[虚拟网络触发器（非 HTTP）](#virtual-network-triggers-non-http)|❌否 |✅是| 
|[混合连接](#hybrid-connections)（仅限 Windows）|❌否 |✅是| 
|[出站 IP 限制](#outbound-ip-restrictions)|❌否 |❌否| 

## <a name="inbound-ip-restrictions"></a>入站 IP 限制

可以使用 IP 限制来定义被允许或拒绝访问应用的 IP 地址的优先级排序列表。 此列表可以包含 IPv4 和 IPv6 地址。 如果存在一个或多个条目，则列表末尾会存在一个隐式的“拒绝所有”。 IP 限制可与所有函数托管选项配合使用。

> [!NOTE]
> 如果进行了网络限制，则只能从虚拟网络内部使用门户编辑器，或者在已将用于访问 Azure 门户的计算机的 IP 地址加入安全收件人列表之后使用该编辑器。 但是，仍可以从任何计算机访问“平台功能”选项卡中的任何功能。 

有关详细信息，请参阅 [Azure 应用服务静态访问限制](../app-service/app-service-ip-restrictions.md)。

## <a name="private-site-access"></a>专用站点访问

专用站点访问指的是使应用仅可供从专用网络（例如 Azure 虚拟网络）访问。

* 配置了服务终结点时，[消耗](functions-scale.md#consumption-plan)和[应用服务](functions-scale.md#app-service-plan)计划中会提供专用站点访问。
    * 可以在“平台功能”   > “网络”   > “配置访问限制”   > “添加规则”  下，针对每个应用配置服务终结点。 现在可以选择虚拟网络作为规则类型。
    * 有关详细信息，请参阅[虚拟网络服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)。
    * 请记住，如果使用服务终结点，那么即使已配置虚拟网络集成，函数也仍对 Internet 拥有完全的出站访问权限。
* 已配置了内部负载均衡器 (ILB) 的应用服务环境内也提供专用站点访问。 有关详细信息，请参阅[在应用服务环境中创建并使用内部负载均衡器](../app-service/environment/create-ilb-ase.md)。

## <a name="virtual-network-integration"></a>虚拟网络集成

函数应用可以通过虚拟网络集成来访问虚拟网络内部的资源。 可在应用服务计划中使用此功能。 如果应用在应用服务环境中，则该应用已处于虚拟网络中，无需虚拟网络集成即可访问同一虚拟网络中的资源。

可以使用虚拟网络集成来从应用访问虚拟网络中运行的数据库和 Web 服务。 使用虚拟网络集成就不需要公开 VM 上应用程序的公共终结点。 可以改用不可通过 Internet 路由的专用地址。

虚拟网络集成有两种形式：

+ **区域虚拟网络集成（预览版）** ：实现与同一区域中的虚拟网络集成。 此类集成需要在同一区域的虚拟网络中有一个子网。 此功能仍为预览版，但受 Windows 上运行的函数应用支持，下面的“问题/解决方案”表后说明了其注意事项。
+ **需要网关的虚拟网络集成**：实现与远程区域中的虚拟网络或经典虚拟网络集成。 这种类型的集成需要将虚拟网关部署到 VNet 中。 这是基于点到站点 VPN 的功能，仅受 Windows 上运行的函数应用支持。

一个应用一次只能使用一种类型的虚拟网络集成功能。 虽然这两种类型均对许多场景十分有用，但下表指出了每种类型应该用于的场景：

| 问题  | 解决方案 |
|----------|----------|
| 想要访问同一区域中的某个 RFC 1918 地址（10.0.0.0/8、172.16.0.0/12、192.168.0.0/16） | 区域虚拟网络集成 |
| 想要访问经典虚拟网络中的资源或另一个区域的虚拟网络中的资源 | 需要网关的虚拟网络集成 |
| 想要通过 Azure ExpressRoute 访问 RFC 1918 终结点 | 区域虚拟网络集成 |
| 想要跨服务终结点访问资源 | 区域虚拟网络集成 |

这两种功能都不允许通过 ExpressRoute 访问非 RFC 1918 地址。 若要执行该操作，目前必须使用应用服务环境。

使用区域虚拟网络集成不会将虚拟网络连接到本地终结点或配置服务终结点。 这是单独的网络配置。 区域虚拟网络集成只是使应用可以跨这些连接类型进行调用。

不管所用版本为何，虚拟网络集成都可让函数应用访问虚拟网络中的资源，但不授权从虚拟网络对函数应用进行专用站点访问。 专用站点访问意味着使应用仅可供从专用网络（例如 Azure 虚拟网络）访问。 虚拟网络集成只是为了进行从应用到虚拟网络内的出站调用。

虚拟网络集成功能：

* 需要标准应用服务计划
* 支持 TCP 和 UDP
* 使用应用服务应用和函数应用

虚拟网络集成不支持某些功能，其中包括：

* 装载驱动器
* Active Directory 集成
* NetBIOS

Azure Functions 中的虚拟网络集成使用与应用服务 Web 应用共享的基础结构。 若要详细了解这两种类型的虚拟网络集成，请参阅：

* [区域虚拟网络集成](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [需要网关的虚拟网络集成](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

## <a name="connecting-to-service-endpoint-secured-resources"></a>连接到服务终结点保护的资源

> [!NOTE]
> 目前，对下游资源配置访问限制后，可能需要多达 12 个小时才能使新的服务终结点对函数应用可用。 在此期间，资源将对应用完全不可用。

为了提供更高级别的安全性，可以使用服务终结点将许多 Azure 服务限制在一个虚拟网络中。 然后，必须将函数应用与该虚拟网络集成，才能访问资源。 支持虚拟网络集成的所有计划都支持此配置。

[详细了解虚拟网络服务终结点。](../virtual-network/virtual-network-service-endpoints-overview.md)

### <a name="restricting-your-storage-account-to-a-virtual-network"></a>将存储帐户限制到虚拟网络

创建函数应用时，必须创建或链接到支持 Blob、队列和表存储的常规用途的 Azure 存储帐户。 目前不能对此帐户使用任何虚拟网络限制。 如果在用于函数应用的存储帐户上配置虚拟网络服务终结点，则会破坏应用。

[详细了解存储帐户要求。](./functions-create-function-app-portal.md#storage-account-requirements)

### <a name="using-key-vault-references"></a>使用 Key Vault 引用 

通过 Key Vault 引用，可以在 Azure Functions 应用程序中使用来自 Azure Key Vault 的机密，而无需进行任何代码更改。 Azure Key Vault 是一项服务，可以提供集中式机密管理，并且可以完全控制访问策略和审核历史记录。

目前，如果你的 Key Vault 通过服务终结点进行保护，[Key Vault 引用](../app-service/app-service-key-vault-references.md)将无法工作。 若要使用虚拟网络集成连接到 Key Vault，需要在应用程序代码中调用密钥保管库。

## <a name="virtual-network-triggers-non-http"></a>虚拟网络触发器（非 HTTP）

目前，若要在虚拟网络中使用不同于 HTTP 的函数触发器，必须在应用服务计划或应用服务环境中运行函数应用。

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>应用服务计划和带有虚拟网络触发器的应用服务环境

当函数应用在应用服务计划或应用服务环境中运行时，可以使用非 HTTP 触发器函数。 为了正确触发函数，必须连接到可访问在触发器连接中定义的资源的虚拟网络。 

例如，假设要将 Azure Cosmos DB 配置为仅接受来自虚拟网络的流量。 在这种情况下，必须在应用服务计划中部署函数应用，该计划提供与该虚拟网络的虚拟网络集成。 这使得函数可由该 Azure Cosmos DB 资源触发。 

## <a name="hybrid-connections"></a>混合连接

[混合连接](../service-bus-relay/relay-hybrid-connections-protocol.md)是可用于访问其他网络中的应用程序资源的一项 Azure 中继功能。 使用混合连接可以从应用访问应用程序终结点。 不能使用混合连接来访问应用程序。 在除消耗计划以外的所有计划中，Windows 上运行的函数可以使用混合连接。

在 Azure Functions 中使用时，每个混合连接与单个 TCP 主机和端口组合相关联。 这意味着，混合连接终结点可以位于任何操作系统和任何应用程序上，前提是你能够访问 TCP 侦听端口。 混合连接功能不知道也不关心应用程序协议或者要访问的内容是什么。 它只提供网络访问。

有关详细信息，请参阅[应用服务文档中的“混合连接”](../app-service/app-service-hybrid-connections.md)。 这些相同的配置步骤支持 Azure Functions。

## <a name="outbound-ip-restrictions"></a>出站 IP 限制

出站 IP 限制仅适用于部署到应用服务环境的函数。 可以为部署了应用服务环境的虚拟网络配置出站限制。

将应用服务计划中的函数应用与虚拟网络集成时，应用仍可对 Internet 进行出站调用。

## <a name="next-steps"></a>后续步骤

若要详细了解网络和 Azure Functions ，请参阅以下链接：

* [阅读 Azure Functions 网络常见问题解答](./functions-networking-faq.md)
* [详细了解虚拟网络与应用服务/Functions 的集成](../app-service/web-sites-integrate-with-vnet.md)
* [详细了解 Azure 中的虚拟网络](../virtual-network/virtual-networks-overview.md)
* [在应用服务环境中允许更多的网络功能和控制](../app-service/environment/intro.md)
* [在不更改防火墙的情况下使用混合连接连接到单个本地资源](../app-service/app-service-hybrid-connections.md)

<!-- Update_Description: wording update -->