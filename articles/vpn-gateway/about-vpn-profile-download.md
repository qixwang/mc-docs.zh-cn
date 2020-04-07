---
title: Azure VPN 网关：关于 P2S VPN 客户端配置文件
description: 这有助于你使用客户端配置文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: article
origin.date: 03/17/2020
ms.date: 04/06/2020
ms.author: v-jay
ms.openlocfilehash: b47814eadc7540987a8c0e2e0b27972b04e3e93f
ms.sourcegitcommit: 5fb45da006859215edc8211481f13174aa43dbeb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80634523"
---
# <a name="about-p2s-vpn-client-profiles"></a>关于 P2S VPN 客户端配置文件

下载的配置文件包含配置 VPN 连接所需的信息。 本文将有助于你获取和了解 VPN 客户端配置文件所需的信息。

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* **OpenVPN 文件夹**包含需要修改以包括密钥和证书的 *ovpn* 配置文件。 有关详细信息，请参阅[为 Azure VPN 网关配置 OpenVPN 客户端](vpn-gateway-howto-openvpn-clients.md#windows)。 如果在 VPN 网关上选择了 Azure AD 身份验证，则此文件夹不会出现在 zip 文件中。 但是，azurevpnconfig.xml 将位于 AzureVPN 文件夹中。

## <a name="next-steps"></a>后续步骤

有关点到站点的详细信息，请参阅[关于点到站点](point-to-site-about.md)。
