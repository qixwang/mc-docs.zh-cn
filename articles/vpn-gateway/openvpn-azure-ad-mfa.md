---
title: 为 VPN 用户启用 MFA：Azure AD 身份验证
description: 为 VPN 用户启用多重身份验证
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: conceptual
origin.date: 11/21/2019
ms.date: 02/10/2020
ms.author: v-jay
ms.openlocfilehash: 553714b6be1689e5d175d5aed2aedbfd42d04c7e
ms.sourcegitcommit: 925c2a0f6c9193c67046b0e67628d15eec5205c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77068582"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>为 VPN 用户启用 Azure 多重身份验证 (MFA)

如果希望在授予访问权限之前提示用户进行第二重身份验证，则可以为每个用户配置 Azure 多重身份验证 (MFA)，或通过条件访问利用 Azure 多重身份验证 (MFA) 实现更细粒度的控制。 可以免费启用配置每用户多重身份验证，但是启用每用户 MFA 时，将提示用户针对绑定到 Azure AD 租户的所有应用程序进行第二重身份验证。 通过条件访问，可以更精细地控制第二个因素的提升方式，并且可以允许将 MFA 仅分配给 VPN，而不分配给绑定到 Azure AD 租户的其他应用程序。

## <a name="enableauth"></a>启用身份验证

1. 导航到“Azure Active Directory”>“企业应用程序”>“所有应用程序”  。
2. 在“企业应用程序 - 所有应用程序”  页面上，选择“Azure VPN”。 

   ![Directory ID](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/user1.jpg)

## <a name="enablesign"></a> 配置登录设置

在“Azure VPN - 属性”  页面上，配置登录设置。

1. 将“启用以供用户登录?”  设置为“是”  。 这允许 AD 租户中的所有用户成功连接到 VPN。
2. 如果希望仅允许对 Azure VPN 具有权限的用户登录，请将“需要进行用户分配?”  设置为“是”  。
3. 保存所做更改。

   ![权限](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="option-1---enable-multi-factor-authentication-mfa-via-conditional-access"></a>选项 1 - 通过条件访问启用 Azure 多重身份验证 (MFA)

条件访问允许基于每个应用程序进行细化的访问控制。  请注意，若要利用条件访问，应将 Azure AD Premium 1 或更高版本的许可应用于将受条件访问规则约束的用户。

1. 在“企业应用程序 - 所有应用程序”  页面上，选择“Azure VPN”  ，选择“条件访问”  ，然后单击“新建策略”  。
2. 在“用户和组”下，在“包括”  选项卡上，选中“选择用户和组”  ，选中“用户和组”  ，然后选择应当进行 MFA 的一个组或一组用户。  单击“完成”  。
![分配](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-assignments.png)
3. 在“授予”  下，选中“授予访问权限”  ，选中“需要多重身份验证”  ，选中“需要所有已选控件”  ，然后单击“选择”  按钮。
![授予访问权限 - MFA](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-grant-mfa.png)
4. 选中“启用策略”下的“开启”，然后单击“创建”按钮。   
![启用策略](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-enable-policy.png)

## <a name="option-2---enable-multi-factor-authentication-mfa-per-user"></a>选项 2 - 启用每用户多重身份验证 (MFA)

[!INCLUDE [MFA steps](../../includes/vpn-gateway-vwan-openvpn-azure-ad-mfa.md)]

## <a name="next-steps"></a>后续步骤

若要连接到虚拟网络，必须创建并配置 VPN 客户端配置文件。 请参阅[配置 VPN 客户端以建立 P2S VPN 连接](openvpn-azure-ad-client.md)。
