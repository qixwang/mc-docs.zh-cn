---
title: '将虚拟网络网关添加到 ExpressRoute 的 VNet：PowerShell：Azure '
description: 本文指导将 VPN 网关添加到已为 ExpressRoute 创建的资源管理器 VNet 中。
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 02/21/2019
ms.author: v-yiso
ms.date: 12/02/2019
ms.openlocfilehash: 6988dbd19bedfe0f639fe521b6c058a511b84019
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74389456"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>使用 PowerShell 配置 ExpressRoute 的虚拟网络网关
> [!div class="op_single_selector"]
> * [Resource Manager - Azure 门户](./expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](./expressroute-howto-add-gateway-resource-manager.md)
> * [经典 - PowerShell](./expressroute-howto-add-gateway-classic.md)
>
>

本文指导你为预先存在的 VNet 添加虚拟网络 (VNet) 网关、重设网关大小以及删除网关。 此配置的步骤适用于使用资源管理器部署模型创建的 VNet（针对 ExpressRoute 配置）。 有关详细信息，请参阅[关于 ExpressRoute 的虚拟网络网关](expressroute-about-virtual-network-gateways.md)。

## <a name="before-beginning"></a>开始之前

### <a name="working-with-powershell"></a>使用 PowerShell

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

### <a name="configuration-reference-list"></a>配置参考列表
[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>后续步骤

创建 VPN 网关之后，可以将 VNet 链接到 ExpressRoute 线路。 请参阅[将虚拟网络链接到 ExpressRoute 线路](./expressroute-howto-linkvnet-arm.md)。