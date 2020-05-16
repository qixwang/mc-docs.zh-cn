---
title: 配置 Always-On VPN 隧道
titleSuffix: Azure Virtual WAN
description: 为虚拟 WAN 配置 Always On VPN 设备隧道的步骤
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: 1cb7bfac1358b1231783af9e6fad038890fd84b2
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002227"
---
<!--Verified successfully on VPN -->
# <a name="configure-an-always-on-vpn-device-tunnel-for-virtual-wan"></a>为虚拟 WAN 配置 Always On VPN 设备隧道

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>先决条件

必须创建点到站点配置并编辑虚拟中心分配。 有关说明，请参阅以下部分：

* [创建 P2S 配置](virtual-wan-point-to-site-portal.md#p2sconfig)
* [编辑中心分配](virtual-wan-point-to-site-portal.md#edit)

## <a name="configure-the-device-tunnel"></a>配置设备隧道

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>删除配置文件

若要删除配置文件，请运行以下命令：

![清理](./media/howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>后续步骤

有关虚拟 WAN 的详细信息，请参阅[常见问题解答](virtual-wan-faq.md)。

<!-- Update_Description: new article about howto always on device tunnel -->
<!--NEW.date: 05/06/2020-->