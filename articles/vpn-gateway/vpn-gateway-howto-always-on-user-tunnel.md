---
title: 配置 Always-On VPN 用户隧道
titleSuffix: Azure VPN Gateway
description: 本文介绍如何为 VPN 网关配置 Always On VPN 用户隧道
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: conceptual
origin.date: 03/12/2020
ms.date: 04/06/2020
ms.author: v-jay
ms.openlocfilehash: 59dd5c28fd815ba640a27669f360d5ef703d342f
ms.sourcegitcommit: 5fb45da006859215edc8211481f13174aa43dbeb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80634515"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>配置 Always On VPN 用户隧道

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>配置网关

 请按照[配置点到站点 VPN 连接](vpn-gateway-howto-point-to-site-resource-manager-portal.md)一文中的说明将 VPN 网关配置为使用 IKEv2 和基于证书的身份验证。

## <a name="configure-a-user-tunnel"></a>配置用户隧道

[!INCLUDE [user configuration](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>删除配置文件

若要删除配置文件，请执行以下步骤：

1. 运行以下命令：

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. 断开连接，清除“自动连接”复选框。 

   ![清理](./media/vpn-gateway-howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>后续步骤

若要排查可能会发生的任何连接问题，请参阅 [Azure 点到站点连接问题](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)。
