---
title: 如何为 Azure VPN 网关配置 OpenVPN 客户端 |Microsoft Docs
description: 为 Azure VPN 网关配置 OpenVPN 客户端的步骤
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: conceptual
origin.date: 12/12/2019
ms.date: 04/06/2020
ms.author: v-jay
ms.openlocfilehash: 92543094c8e1fa22299b1ce13317cc16bcae7bb2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80634409"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>为 Azure VPN 网关配置 OpenVPN 客户端

本文可帮助你配置 OpenVPN  **协议&reg;** 客户端。

## <a name="before-you-begin"></a>开始之前

验证你已完成为 VPN 网关配置 OpenVPN 的步骤。 有关详细信息，请参阅[为 Azure VPN 网关配置 OpenVPN](vpn-gateway-howto-openvpn.md)。

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>后续步骤

如果希望 VPN 客户端能够访问另一个 VNet 中的资源，请遵照 [VNet 到 VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) 文章中的说明设置 VNet 到 VNet 连接。 确保在网关和连接中启用 BGP，否则流量不会流动。

**“OpenVPN”是 OpenVPN Inc. 的商标。**
