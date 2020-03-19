---
title: 配置 Azure 虚拟 WAN 的自定义 IPsec 策略：门户 | Azure
description: 了解如何使用门户为 Azure 虚拟 WAN 配置自定义 IPsec 策略。
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 10/14/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: b5a43843fc8dd7c3cf7b4f3a4459b26396b0002a
ms.sourcegitcommit: e94ed1c9eff4e88be2ca389909e60b14cc0d92f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084448"
---
# <a name="configure-a-custom-ipsec-policy-for-virtual-wan-using-the-portal"></a>使用门户为虚拟 WAN 配置自定义 IPsec 策略

可以在 Azure 门户中为虚拟 WAN 配置自定义 IPsec 策略。 如果希望两端（本地和 Azure VPN 网关）对“IKE 阶段 1”和“IKE 阶段 2”使用相同的设置，则可使用自定义策略。

## <a name="working-with-custom-policies"></a>使用自定义策略

[!INCLUDE [IPsec](../../includes/virtual-wan-ipsec-custom-include.md)]

## <a name="configure-a-policy"></a>配置策略

1. **找到虚拟中心**。 从浏览器导航到 [Azure 门户](https://portal.azure.cn/?feature.canmodifystamps=true&Microsoft_Azure_Network=flight16&feature.vwaner=true&feature.vwanp2s=true)并使用 Azure 帐户登录。 找到站点的虚拟中心。
2. **选择 VPN 站点**。 在中心页上，选择要为其设置自定义策略的 VPN 站点。

    ![select](./media/virtual-wan-custom-ipsec-portal/locate.png)
3. **编辑 VPN 连接**。 从上下文菜单“...”中，选择“编辑 VPN 连接”    。

    ![编辑](./media/virtual-wan-custom-ipsec-portal/contextmenu.png)
4. **配置设置**。 在“编辑 VPN 连接”  页上，配置设置。 选择“保存”以保存设置  。

    ![配置并保存](./media/virtual-wan-custom-ipsec-portal/edit.png)

## <a name="next-steps"></a>后续步骤

若要详细了解虚拟 WAN，请参阅[虚拟 WAN 概述](virtual-wan-about.md)页。

<!-- Update_Description: new article about virtual wan custom ipsec portal -->
<!--NEW.date: 03/09/2020-->