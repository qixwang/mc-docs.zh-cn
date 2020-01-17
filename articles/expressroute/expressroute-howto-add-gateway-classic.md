---
title: '使用 PowerShell 为 ExpressRoute 配置 VPN 网关: 经典：Azure '
description: 在 ExpressRoute 配置中使用 PowerShell 配置经典部署模型 VNet 的 VPN 网关。
documentationcenter: na
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 12/06/2019
ms.date: 01/20/2020
ms.author: v-yiso
ms.openlocfilehash: e773980b122a3ba7d2c20f0598d1bc96baa8de12
ms.sourcegitcommit: a890a9cca495d332c9f3f53ff3a5259fd5f0c275
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75859582"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>使用 PowerShell 配置 ExpressRoute 的虚拟网络网关（经典）
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](./expressroute-howto-add-gateway-resource-manager.md)
> * [经典 - PowerShell](./expressroute-howto-add-gateway-classic.md)
>
>

本文指导你完成为预先存在的 VNet 添加、重设大小和删除虚拟网络 (VNet) 网关的步骤。 此配置的步骤专用于使用**经典部署模型**创建的、会在 ExpressRoute 配置中使用的 VNet。 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**关于 Azure 部署模型**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>开始之前
确认已安装此配置所需的 Azure PowerShell cmdlet。

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>后续步骤

创建 VPN 网关之后，可以将 VNet 链接到 ExpressRoute 线路。 请参阅[将虚拟网络链接到 ExpressRoute 线路](./expressroute-howto-linkvnet-classic.md)。