---
title: 为 VPN 用户启用 MFA：Azure AD 身份验证
description: 为 VPN 用户启用多重身份验证
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 01/16/2020
ms.date: 03/30/2020
ms.author: v-yeche
ms.openlocfilehash: 266c01e687a5e0e4693e4836493fc8e8abd6b8be
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80291312"
---
<!--Verified successfully-->

# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>为 VPN 用户启用 Azure 多重身份验证 (MFA)

[!INCLUDE [overview](../../includes/vpn-gateway-vwan-openvpn-enable-mfa-overview.md)]

<a name="enableauth"></a>
## <a name="enable-authentication"></a>启用身份验证

[!INCLUDE [enable authentication](../../includes/vpn-gateway-vwan-openvpn-enable-auth.md)]

<a name="enablesign"></a>
## <a name="configure-sign-in-settings"></a>配置登录设置

[!INCLUDE [sign in](../../includes/vpn-gateway-vwan-openvpn-sign-in.md)]

<a name="peruser"></a>
## <a name="option-1---per-user-access"></a>选项 1 - 每用户访问

[!INCLUDE [per user](../../includes/vpn-gateway-vwan-openvpn-per-user.md)]

<!--Not Available on ## Option 2 - Conditional Access-->

## <a name="next-steps"></a>后续步骤

若要连接到虚拟网络，必须创建并配置 VPN 客户端配置文件。 请参阅[为与 Azure 的点到站点连接配置 Azure AD 身份验证](virtual-wan-point-to-site-azure-ad.md)。

<!-- Update_Description: new article about openvpn azure ad mfa -->
<!--NEW.date: 02/24/2020-->