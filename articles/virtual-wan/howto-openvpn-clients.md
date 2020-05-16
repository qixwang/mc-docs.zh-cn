---
title: 为 Azure 虚拟 WAN 配置 OpenVPN 客户端
description: 为 Azure 虚拟 WAN 配置 OpenVPN 客户端的步骤
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: 75b38bc56ed8b71c51137f5ce142d55e49b5535b
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002232"
---
<!--Verified successfully on VPN OpenVPN client-->
# <a name="configure-an-openvpn-client-for-azure-virtual-wan"></a>为 Azure 虚拟 WAN 配置 OpenVPN 客户端

本文可帮助你配置 OpenVPN &reg; 协议  客户端。

## <a name="before-you-begin"></a>准备阶段

创建用户 VPN（点到站点）配置。 请确保选择“OpenVPN”作为隧道类型。 有关步骤，请参阅[为 Azure 虚拟 WAN 创建 P2S 配置](virtual-wan-point-to-site-portal.md#p2sconfig)。

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>后续步骤

有关用户 VPN（点到站点）的详细信息，请参阅[创建用户 VPN 连接](virtual-wan-point-to-site-portal.md)。

**“OpenVPN”是 OpenVPN Inc. 的商标。**


<!-- Update_Description: new article about howto openvpn clients -->
<!--NEW.date: 05/06/2020-->