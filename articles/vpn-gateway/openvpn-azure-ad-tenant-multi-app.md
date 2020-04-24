---
title: VPN 网关：不同用户组的 Azure AD 租户：Azure AD 身份验证
description: 可以使用 P2S VPN 通过 Azure AD 身份验证连接到 VNet
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: conceptual
origin.date: 02/19/2020
ms.date: 04/06/2020
ms.author: v-jay
ms.openlocfilehash: df2850c0257bb16af477da6993cc498080b2225a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80634584"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>为 P2S OpenVPN 协议连接创建 Azure Active Directory 租户

连接到 VNet 时，可以使用基于证书的身份验证或 RADIUS 身份验证。 但是，在使用开放 VPN 协议时，还可以使用 Azure Active Directory 身份验证。 如果你希望一组不同的用户能够连接到不同的 VPN 网关，可以在 AD 中注册多个应用，并将这些应用链接到不同的 VPN 网关。 本文帮助你设置用于 P2S OpenVPN 身份验证的 Azure AD 租户，并在 Azure AD 中创建和注册多个应用，使不同的用户和组能够以不同的方式进行访问。

> [!NOTE]
> Azure AD 身份验证仅支持用于 OpenVPN®协议连接。
>

[!INCLUDE [create](../../includes/openvpn-azure-ad-tenant-multi-app.md)]

## <a name="6-enable-authentication-on-the-gateway"></a><a name="enable-authentication"></a>6.在网关上启用身份验证

在此步骤中，将在 VPN 网关上启用 Azure AD 身份验证。

1. 通过运行以下命令，在 VPN 网关上启用 Azure AD 身份验证。 请确保修改这些命令以反映自己的环境：

    ```azurepowershell
    $gw = Get-AzVirtualNetworkGateway -Name <name of VPN gateway> -ResourceGroupName <Resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientRootCertificates @()
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -AadTenantUri "https://login.partner.microsoftonline.cn/<your Directory ID>" -AadAudienceId "application ID from previous section" -AadIssuerUri "https://sts.chinacloudapi.cn/<your Directory ID>/" -VpnClientAddressPool 192.168.0.0/24
    ```
    > [!NOTE]
    > 请不要在上述命令中使用 Azure VPN 客户端的应用程序 ID：它会向所有用户授予对 VPN 网关的访问权限。 使用注册的应用程序的 ID。

2. 运行以下命令来创建并下载配置文件。 请更改 -ResourcGroupName 和 -Name 值，使之与你自己的值匹配。

    ```azurepowershell
    $profile = New-AzVpnClientConfiguration -Name <name of VPN gateway> -ResourceGroupName <Resource group> -AuthenticationMethod "EapTls"
    $PROFILE.VpnProfileSASUrl
    ```

3. 运行这些命令后，会看到如下所示的结果。 将结果 URL 复制到浏览器，以下载 zip 配置文件。

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/profile.png)

4. 解压缩已下载的 zip 文件。

5. 浏览到解压缩后的“AzureVPN”文件夹。

6. 记下“azurevpnconfig.xml”文件的位置。 azurevpnconfig.xml 包含 VPN 连接的设置，可以直接导入到 Azure VPN 客户端应用程序中。 还可以将此文件分发给需要通过电子邮件或其他方式建立连接的所有用户。 用户需有有效的 Azure AD 凭据才能成功建立连接。

## <a name="next-steps"></a>后续步骤

若要连接到虚拟网络，必须创建并配置 VPN 客户端配置文件。 请参阅[配置 VPN 客户端以建立 P2S VPN 连接](openvpn-azure-ad-client.md)。
