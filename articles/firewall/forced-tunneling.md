---
title: Azure 防火墙强制隧道
description: 你可以配置强制隧道来将 Internet 绑定的流量路由到其他防火墙或网络虚拟设备，以便进一步处理。
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: article
origin.date: 02/24/2020
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: 78e3b7526f4ab7f7d7fdbeba8312892c695b0bbf
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001936"
---
<!--Verified successfully-->
# <a name="azure-firewall-forced-tunneling-preview"></a>Azure 防火墙强制隧道（预览版）

你可以对 Azure 防火墙进行配置，使其将所有 Internet 绑定的流量路由到指定的下一跃点，而不是直接前往 Internet。 例如，你可能有本地边缘防火墙或其他网络虚拟设备 (NVA) 用来在将网络流量传递到 Internet 之前对其进行处理。

> [!IMPORTANT]
> Azure 防火墙强制隧道目前为公共预览版。
>
> 此公共预览版在提供时没有附带服务级别协议，不应该用于生产工作负荷。 某些功能可能不受支持或受到约束，或者不一定在所有 Azure 位置都可用。 有关详细信息，请参阅[适用于 Azure 预览版的补充使用条款](https://www.azure.cn/support/legal/subscription-agreement/)。

默认情况下，Azure 防火墙不允许强制隧道，以确保满足所有的出站 Azure 依赖关系。 *AzureFirewallSubnet* 上其中的默认路由未直接前往 Internet 的用户定义路由 (UDR) 配置处于禁用状态。

## <a name="forced-tunneling-configuration"></a>强制隧道配置

若要支持强制隧道，服务管理流量需要与客户流量分离开来。 需要有一个额外的名为 *AzureFirewallManagementSubnet* 的专用子网（最小子网大小 /26），并且它必须有其自己的关联公共 IP 地址。 此子网上允许的唯一路由是到 Internet 的默认路由，并且必须禁用 BGP 路由传播。

如果你有通过 BGP 播发的默认路由强制将流量传输到本地，则必须在部署防火墙之前创建 *AzureFirewallSubnet* 和 *AzureFirewallManagementSubnet*，必须有一个 UDR 包含到 Internet 的默认路由，并且必须禁用“虚拟网络网关路由传播”  。

在此配置中，*AzureFirewallSubnet* 现在可以包括到任何本地防火墙或 NVA 的路由，以便在将流量传递到 Internet 之前对其进行处理。 如果在此子网上启用了“虚拟网络网关路由传播”  ，则还可以通过 BGP 将这些路由发布到 *AzureFirewallSubnet*。

例如，你可以在 *AzureFirewallSubnet* 上创建一个默认路由并使用你的 VPN 网关作为下一跃点，以到达你的本地设备。 还可以启用“虚拟网络网关路由传播”  以获得通向本地网络的合适路由。

![虚拟网络网关路由传播](media/forced-tunneling/route-propagation.png)

将 Azure 防火墙配置为支持强制隧道后，无法撤消此配置。 如果删除防火墙上的所有其他 IP 配置，则会同时删除管理 IP 配置，并且会将防火墙解除分配。 无法删除分配给管理 IP 配置的公共 IP 地址，但可以分配一个不同的公共 IP 地址。

## <a name="next-steps"></a>后续步骤

- [教程：使用 Azure 门户在混合网络中部署和配置 Azure 防火墙](tutorial-hybrid-portal.md)

<!-- Update_Description: new article about forced tunneling -->
<!--NEW.date: 03/09/2020-->