---
title: 为 VPN 用户启用 MFA：Azure AD 身份验证
description: 为 VPN 用户启用多重身份验证
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 01/16/2020
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: 2a1e6650ebf6da6a41a1ad96764e7b0cb59ad1fe
ms.sourcegitcommit: 69cadf1fa0ed81751c48fbce919a6bb44b1053ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209193"
---
<!--Verified successfully-->

# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>为 VPN 用户启用 Azure 多重身份验证 (MFA)

如果希望在授予访问权限之前提示用户提供第二个身份验证因素，可以为 Azure AD 租户配置 Azure 多重身份验证 (MFA)。 本文中的步骤可帮助你启用双重验证要求。

<a name="prereq"></a>
## <a name="prerequisite"></a>先决条件

此配置的先决条件是使用[配置租户](openvpn-azure-ad-tenant.md)中的步骤配置 Azure AD 租户。

[!INCLUDE [MFA steps](../../includes/vpn-gateway-vwan-openvpn-azure-ad-mfa.md)]

<a name="enablesign"></a>
## <a name="configure-sign-in-settings"></a>配置登录设置

在“Azure VPN - 属性”  页面上，配置登录设置。

1. 将“启用以供用户登录?”  设置为“是”  。 这允许 AD 租户中的所有用户成功连接到 VPN。
2. 如果希望仅允许对 Azure VPN 具有权限的用户登录，请将“需要进行用户分配?”  设置为“是”  。
3. 保存所做更改。

    ![权限](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="next-steps"></a>后续步骤

若要连接到虚拟网络，必须创建并配置 VPN 客户端配置文件。 请参阅[为与 Azure 的点到站点连接配置 Azure AD 身份验证](virtual-wan-point-to-site-azure-ad.md)。

<!-- Update_Description: new article about openvpn azure ad mfa -->
<!--NEW.date: 02/24/2020-->