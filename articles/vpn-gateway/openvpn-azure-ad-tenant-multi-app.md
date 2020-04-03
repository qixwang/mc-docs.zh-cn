---
title: VPN 网关：不同用户组的 Azure AD 租户：Azure AD 身份验证
description: 可以使用 P2S VPN 通过 Azure AD 身份验证连接到 VNet
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: conceptual
origin.date: 12/20/2019
ms.date: 02/10/2020
ms.author: v-jay
ms.openlocfilehash: bac2c121a37375d45010e23a4c55127fa4bdf6fa
ms.sourcegitcommit: f204ec3ae220b96475db60e6ac8a2807af3de40e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80418029"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>为 P2S OpenVPN 协议连接创建 Azure Active Directory 租户

连接到 VNet 时，可以使用基于证书的身份验证或 RADIUS 身份验证。 但是，在使用开放 VPN 协议时，还可以使用 Azure Active Directory 身份验证。 如果你希望一组不同的用户能够连接到不同的 VPN 网关，可以在 AD 中注册多个应用，并将这些应用链接到不同的 VPN 网关。 本文帮助你设置用于 P2S OpenVPN 身份验证的 Azure AD 租户，并在 Azure AD 中创建和注册多个应用，使不同的用户和组能够以不同的方式进行访问。

> [!NOTE]
> Azure AD 身份验证仅支持用于 OpenVPN®协议连接。
>

## <a name="1-create-the-azure-ad-tenant"></a><a name="tenant"></a>1.创建 Azure AD 租户

使用[创建新租户](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)一文中的步骤创建 Azure AD 租户：

* 组织名称
* 初始域名

示例：

   ![新 Azure AD 租户](./media/openvpn-azure-ad-tenant-multi-app/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2.创建 Azure AD 租户用户

接下来，创建两个用户帐户。 创建一个全局管理员帐户和一个主要用户帐户。 主要用户帐户用作主要嵌入帐户（服务帐户）。 创建 Azure AD 租户用户帐户时，可以根据要创建的用户类型调整目录角色。

使用[此文](../active-directory/fundamentals/add-users-azure-active-directory.md)中的步骤为 Azure AD 租户创建至少两个用户。 若要创建帐户类型，请务必更改“目录角色”： 

* 全局管理员
* User

## <a name="3-register-azure-vpn-client-in-azure-ad-tenant"></a><a name="enable-authentication"></a>3.在 Azure AD 租户中注册 Azure VPN 客户端

1. 找到要用于身份验证的目录的目录 ID。 此 ID 在“Active Directory”页的“属性”部分中列出。

    ![Directory ID](./media/openvpn-azure-ad-tenant-multi-app/directory-id.png)

2. 复制“目录 ID”。

3. 以拥有“全局管理员”角色的用户身份登录到 Azure 门户。 

4. 接下来，做出管理员许可。 在浏览器的地址栏中复制并粘贴与部署位置相关的 URL：

    Azure 中国世纪互联

    ```
    https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. 出现提示时，请选择“全局管理员”帐户。 

    ![Directory ID](./media/openvpn-azure-ad-tenant-multi-app/pick.png)

6. 出现提示时，请选择“接受”。 

    ![Accept](./media/openvpn-azure-ad-tenant-multi-app/accept.jpg)

7. 在 Azure AD 下的“企业应用程序”中，将会发现已列出“Azure VPN”。  

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/azurevpn.png)

## <a name="4-register-additional-applications-for-various-usersgroups"></a><a name="enable-authentication"></a>4.为各个用户/组注册其他应用程序

1. 在 Azure Active Directory 下，依次单击“应用注册”、“+ 新建注册”  

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app1.png)

2. 在“注册应用程序”边栏选项卡中输入“名称”字段，选择所需的“支持的帐户类型”，然后单击“注册”    

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app2.png)

3. 注册新应用后，单击应用边栏选项卡下的“公开 API” 

4. 单击“+ 添加范围” 
5. 保留默认的“应用程序 ID URI”，并单击“保存并继续”  

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app3.png)
6. 填写必填字段，确保“状态”为“已启用”。   单击“添加范围” 

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app4.png)
7. 依次单击“公开 API”、“+ 添加客户端应用程序”。    对于“客户端 ID”，请根据云输入以下值： 
    -   对于“Azure 中国世纪互联”，请输入 **49f817b6-84ae-4cc0-928c-73f27289b3aa** 


8. 单击“添加应用程序” 

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app5.png)
9. 复制“概述”页中的“应用程序(客户端) ID”。   需要使用此 ID 来配置 VPN 网关

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app6.png)

10. 重复本部分 (4) 中的步骤，根据安全要求创建所需数量的应用程序。 每个应用程序将关联到某个 VPN 网关，并可以包含一组不同的用户。 只能将一个应用程序关联到一个网关。

## <a name="5-assign-users-to-your-applications"></a><a name="enable-authentication"></a>5.将用户分配到应用程序

1. 在 Azure AD 下的“企业应用程序”中，选择新注册的应用程序并单击“属性”。   确保“需要用户分配?”设置为“是”。   单击“保存” 

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user2.png)

2. 在应用页上，依次单击“用户和组”、“添加用户”  

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user3.png)
3. 在“添加分配”下，单击“用户和组”。   选择希望其能够访问此 VPN 应用程序的用户。 单击“选择” 

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user4.png)

## <a name="6-enable-azure-ad-authentication-on-the-vpn-gateway"></a><a name="enable-authentication"></a>6.在 VPN 网关上启用 Azure AD 身份验证

1. 运行以下命令（请务必修改命令来反映自己的环境），在 VPN 网关上启用 Azure AD 身份验证：

    ```azurepowershell
    $gw = Get-AzVirtualNetworkGateway -Name <name of VPN gateway> -ResourceGroupName <Resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientRootCertificates @()
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -AadTenantUri "https://login.partner.microsoftonline.cn/<your Directory ID>" -AadAudienceId "application ID from previous section" -AadIssuerUri "https://sts.chinacloudapi.cn/<your Directory ID>/" -VpnClientAddressPool 192.168.0.0/24
    ```
> [!NOTE]
> 在上述命令中，请不要使用 Azure VPN 客户端的应用程序 ID，因为它会向所有用户授予对 VPN 网关的访问权限。 使用注册的应用程序的 ID。

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
