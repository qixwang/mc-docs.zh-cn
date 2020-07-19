---
title: Azure 文件存储的网络注意事项 | Microsoft Docs
description: Azure 文件存储的网络选项概述。
author: WenJason
ms.service: storage
ms.topic: overview
origin.date: 02/22/2020
ms.date: 07/20/2020
ms.author: v-jay
ms.subservice: files
ms.openlocfilehash: 14224566f566aa91c1e56ad4633158f591368cf4
ms.sourcegitcommit: 31da682a32dbb41c2da3afb80d39c69b9f9c1bc6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2020
ms.locfileid: "86414588"
---
# <a name="azure-files-networking-considerations"></a>Azure 文件存储的网络注意事项 
可以通过一种方式连接到 Azure 文件共享：

直接通过 SMB 或 FileREST 协议访问共享。 要删除尽可能多的本地服务器时，主要使用此访问模式。

本文重点介绍如何在用例要求直接访问 Azure 文件共享时配置网络。

Azure 文件共享的网络配置是在 Azure 存储帐户中完成的。 存储帐户是代表共享存储池的管理结构，你可以在其中部署多个文件共享以及其他存储资源（例如，Blob 容器或队列）。 存储帐户公开多种设置用于帮助保护对文件共享的网络访问：网络终结点、存储帐户防火墙设置和传输中加密。 

在阅读本概念指南之前，我们建议先阅读[规划 Azure 文件存储部署](storage-files-planning.md)。

## <a name="accessing-your-azure-file-shares"></a>访问 Azure 文件共享
在存储帐户中部署 Azure 文件共享时，可以通过该存储帐户的公共终结点立即访问该文件共享。 这意味着，已经过身份验证的请求（例如已由用户登录标识授权的请求）可以安全地从 Azure 内部或外部发起。 

在许多客户环境中，最初在本地工作站上装载 Azure 文件共享的操作会失败，尽管可以成功地从 Azure VM 装载。 其原因是，许多组织和 Internet 服务提供商 (ISP) 阻止 SMB 用来通信的端口 445。 这种做法源自于有关传统版和已弃用版 SMB 协议的安全指导原则。 SMB 3.0 是 Internet 安全的协议，但早期版本的 SMB，尤其是 SMB 1.0，却并非如此。 在外部，只能使用公共终结点通过 SMB 3.0 和 FileREST 协议（也是 Internet 安全的协议）访问 Azure 文件共享。

由于从本地访问 Azure 文件共享的最简单方法是在本地网络中开放端口 445，因此 Azure 建议使用以下步骤从环境中删除 SMB 1.0：

1. 请确认是否在组织的设备上删除或禁用了 SMB 1.0。 Windows 和 Windows Server 当前支持的所有版本均支持删除或禁用 SMB 1.0，并且自 Windows 10 1709 版起，默认情况下 Windows 上未安装 SMB 1.0。 若要详细了解如何禁用 SMB 1.0，请参阅特定于 OS 的页面：
    - [保护 Windows/Windows Server](storage-how-to-use-files-windows.md#securing-windowswindows-server)
    - [保护 Linux](storage-how-to-use-files-linux.md#securing-linux)
2. 请确认组织内没有需要 SMB 1.0 的产品并将其删除。 我们提供 [SMB1 产品交换所](https://aka.ms/stillneedssmb1)，其中包含需要 SMB 1.0 的所有 Microsoft 已知的第一和第三方产品。 
3. （可选）在组织的本地网络中使用第三方防火墙，以阻止 SMB 1.0 流量离开组织边界。

如果组织要求按照政策或法规阻止端口 445，或者组织要求发往 Azure 的流量遵循确定性的路径，则你可以使用 Azure VPN 网关或 ExpressRoute 将流量以隧道方式传输到 Azure 文件共享。

> [!Important]  
> 即使你决定使用替代方法来访问 Azure 文件共享，Azure 也仍建议从环境中删除 SMB 1.0。

### <a name="tunneling-traffic-over-a-virtual-private-network-or-expressroute"></a>通过虚拟专用网络或 ExpressRoute 以隧道方式传输流量
在本地网络与 Azure 之间建立网络隧道时，会将本地网络对等互连到 Azure 中的一个或多个虚拟网络。 [虚拟网络](../../virtual-network/virtual-networks-overview.md) (VNet) 类似于在本地运行的传统网络。 与 Azure 存储帐户或 Azure VM 一样，VNet 是在资源组中部署的 Azure 资源。 

Azure 文件存储支持通过以下机制在本地工作站和服务器与 Azure 之间以隧道方式传输流量：

- [Azure VPN 网关](../../vpn-gateway/vpn-gateway-about-vpngateways.md)：VPN 网关是特定类型的虚拟网关，用于通过 Internet 在 Azure 虚拟网络和备用位置（例如，本地）之间发送加密的流量。 Azure VPN 网关是一种 Azure 资源，可以与存储帐户或其他 Azure 资源一起部署在资源组中。 VPN 网关公开了两种不同类型的连接：
    - [点到站点 (P2S) VPN](../../vpn-gateway/point-to-site-about.md) 网关连接，这是 Azure 与单个客户端之间建立的 VPN 连接。 对于那些不属于组织的本地网络的设备（例如，希望能够在家中、咖啡店或酒店随时随地装载 Azure 文件共享的远程办公人员），此解决方案非常有用。 若要将 P2S VPN 连接与 Azure 文件存储一起使用，需要为每个要连接的客户端配置 P2S VPN 连接。 
    - [站点到站点 (S2S) VPN](../../vpn-gateway/design.md#s2smulti)，这是 Azure 与组织的网络之间建立的 VPN 连接。 通过 S2S VPN 连接，你可以为组织的网络上托管的 VPN 服务器或设备一次性配置 VPN 连接，而不是为需要访问 Azure 文件共享的每个客户端设备都进行一次配置。
- [ExpressRoute](../../expressroute/expressroute-introduction.md)，使你可以在 Azure 与不经过 Internet 的本地网络之间创建定义的路由。 因为 ExpressRoute 在本地数据中心和 Azure 之间提供了专用路径，所以当存在网络性能方面的顾虑时，ExpressRoute 可能会很有用。 组织的策略或法规要求使用确定的路径访问云中的资源时，ExpressRoute 也是一个不错的选择。

无论使用哪种隧道方法来访问 Azure 文件共享，都需要通过某种机制来确保发往存储帐户的流量经过隧道，而不是经过普通的 Internet 连接。 在技术上可以将流量路由到存储帐户的公共终结点，但这需要对区域中 Azure 存储群集的所有 IP 地址进行硬编码，因为存储帐户随时都可能在存储群集之间移动。 此外，还需要经常更新 IP 地址映射，因为新群集在不断地添加。

## <a name="storage-account-firewall-settings"></a>存储帐户防火墙设置
防火墙是一种网络策略，控制允许哪些请求访问存储帐户的公共终结点。 使用存储帐户防火墙，可以仅限特定的 IP 地址、IP 范围或虚拟网络访问存储帐户的公共终结点。 通常，大多数针对存储帐户的防火墙策略仅限一个或多个虚拟网络进行网络访问。 

可通过一种方法来仅限虚拟网络访问存储帐户：仅限一个或多个虚拟网络访问公共终结点。 为此，可以使用称作“服务终结点”的虚拟网络功能。 通过服务终结点限制发往存储帐户的流量时，仍会通过公共 IP 地址访问存储帐户。

若要详细了解如何配置存储帐户防火墙，请参阅[配置 Azure 存储防火墙和虚拟网络](../common/storage-network-security.md?toc=%2fstorage%2ffiles%2ftoc.json)。

## <a name="encryption-in-transit"></a>传输中加密
默认情况下，所有 Azure 存储帐户均已启用传输中加密。 即通过 SMB 装载文件共享或通过 FileREST 协议（例如，通过 Azure门户、PowerShell/CLI 或 Azure SDK）访问文件共享时，Azure 文件存储仅允许通过加密或 HTTPS 使用 SMB 3.0 及更高版本建立的连接。 如果启用了传输中加密，则不支持 SMB 3.0 的客户端或支持 SMB 3.0 但不支持 SMB 加密的客户端将无法装载 Azure 文件共享。 要详细了解哪些操作系统支持具有加密功能的 SMB 3.0，请参阅适用于 [Windows](storage-how-to-use-files-windows.md)、[macOS](storage-how-to-use-files-mac.md) 和 [Linux](storage-how-to-use-files-linux.md) 的详细文档。 PowerShell、CLI 和 SDK 的所有当前版本均支持 HTTPS。  

可以为 Azure 存储帐户禁用传输中加密。 禁用加密后，Azure 文件存储还将允许没有加密功能的 SMB 2.1、SMB 3.0 和通过 HTTP 进行的未经加密的 FileREST API 调用。 禁用传输中加密的主要原因是为了支持必须在更低版本的操作系统（例如，Windows Server 2008 R2 或更低版本的 Linux 发行版）上运行的旧版应用程序。 Azure 文件存储仅允许在与 Azure 文件共享相同的 Azure 区域内建立 SMB 2.1 连接；Azure 文件共享的 Azure 区域之外的 SMB 2.1 客户端（例如，本地或其他 Azure 区域）将无法访问文件共享。

有关传输中加密的详细信息，请参阅[要求在 Azure 存储中进行安全传输](../common/storage-require-secure-transfer.md?toc=%2fstorage%2ffiles%2ftoc.json)。

## <a name="see-also"></a>另请参阅
- [Azure 文件存储概述](storage-files-introduction.md)
- [规划 Azure 文件部署](storage-files-planning.md)