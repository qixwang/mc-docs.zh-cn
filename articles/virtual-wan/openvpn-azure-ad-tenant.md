---
title: VPN 网关：用于 P2S VPN 连接的 Azure AD 租户：Azure AD 身份验证
description: 可以使用 P2S VPN 通过 Azure AD 身份验证连接到 VNet
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 12/27/2019
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: 608fbf57f33bc59fcf8a72c7192f7b96733f743d
ms.sourcegitcommit: 564739de7e63e19a172122856ebf1f2f7fb4bd2e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82093479"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>为 P2S OpenVPN 协议连接创建 Azure Active Directory 租户

连接到 VNet 时，可以使用基于证书的身份验证或 RADIUS 身份验证。 但是，在使用开放 VPN 协议时，还可以使用 Azure Active Directory 身份验证。 本文帮助设置用于 P2S Open VPN 身份验证的 Azure AD 租户。

> [!NOTE]
> Azure AD 身份验证仅支持用于 OpenVPN®协议连接。
>

<a name="tenant"></a>
## <a name="1-create-the-azure-ad-tenant"></a>1.创建 Azure AD 租户

使用[创建新租户](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)一文中的步骤创建 Azure AD 租户：

* 组织名称
* 初始域名

    示例：

    ![新 Azure AD 租户](./media/openvpn-create-azure-ad-tenant/newtenant.png)

<a name="users"></a>
## <a name="2-create-azure-ad-tenant-users"></a>2.创建 Azure AD 租户用户

接下来，创建两个用户帐户。 创建一个全局管理员帐户和一个主要用户帐户。 主要用户帐户用作主要嵌入帐户（服务帐户）。 创建 Azure AD 租户用户帐户时，可以根据要创建的用户类型调整目录角色。

使用[此文](../active-directory/fundamentals/add-users-azure-active-directory.md)中的步骤为 Azure AD 租户创建至少两个用户。 若要创建帐户类型，请务必更改“目录角色”： 

* 全局管理员
* User

<a name="enable-authentication"></a>
## <a name="3-enable-azure-ad-authentication-on-the-vpn-gateway"></a>3.在 VPN 网关上启用 Azure AD 身份验证

1. 找到要用于身份验证的目录的目录 ID。 此 ID 在“Active Directory”页的“属性”部分中列出。

    ![Directory ID](./media/openvpn-create-azure-ad-tenant/directory-id.png)

2. 复制“目录 ID”。

3. 以拥有“全局管理员”角色的用户身份登录到 Azure 门户。 

4. 接下来，做出管理员许可。 在浏览器的地址栏中复制并粘贴与部署位置相关的 URL：

    <!--CORRECT ON THE SIGN IN URL-->
    
    公共

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Government

    ```
    https://login-us.microsoftonline.com/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    德国 Microsoft 云

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````
    
    <!--CORRECT ON THE SIGN IN URL-->
    
    Azure 中国世纪互联

    ```
    https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. 出现提示时，请选择“全局管理员”帐户。 

    ![Directory ID](./media/openvpn-create-azure-ad-tenant/pick.png)

6. 出现提示时，请选择“接受”。 

    ![Accept](./media/openvpn-create-azure-ad-tenant/accept.jpg)

7. 在 Azure AD 下的“企业应用程序”中，将会发现已列出“Azure VPN”。  

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/azurevpn.png)

8. 按照[为与 Azure 的点到站点连接配置 Azure AD 身份验证](virtual-wan-point-to-site-azure-ad.md)中的步骤，为用户 VPN 配置 Azure AD 身份验证并将其分配给虚拟中心

## <a name="next-steps"></a>后续步骤

若要连接到虚拟网络，必须创建并配置 VPN 客户端配置文件，并将其与虚拟中心关联。 请参阅[为与 Azure 的点到站点连接配置 Azure AD 身份验证](virtual-wan-point-to-site-azure-ad.md)。

<!-- Update_Description: new article about openvpn azure ad tenant -->
<!--NEW.date: 02/24/2020-->