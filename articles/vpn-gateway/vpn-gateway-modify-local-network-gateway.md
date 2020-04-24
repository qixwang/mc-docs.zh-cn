---
title: 修改本地网络网关 IP 地址前缀和 VPN 网关 IP 地址 | Azure| PowerShell| Microsoft Docs
description: 本文逐步介绍了如何使用 PowerShell 更改本地网关的 IP 地址前缀
services: vpn-gateway
documentationcenter: na
author: WenJason
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 06/19/2017
ms.date: 12/10/2018
ms.author: v-jay
ms.openlocfilehash: 84ab29ad0f3bdf05bcd4279bf91c55cdfe62e567
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63851162"
---
# <a name="modify-local-network-gateway-settings-using-powershell"></a>使用 PowerShell 修改本地网络网关设置

有时本地网络网关 AddressPrefix 或 GatewayIPAddress 的设置会变更。 本文演示如何修改本地网络网关设置。 还可以选择以下列表中的其他选项，使用另一种方法来修改这些设置：

> [!div class="op_single_selector"]
> * [Azure 门户](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before-you-begin"></a><a name="before"></a>准备工作

安装最新版本的 Azure 资源管理器 PowerShell cmdlet。 有关安装 PowerShell cmdlet 的详细信息，请参阅 [如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs) 。

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>修改 IP 地址前缀

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>修改网关 IP 地址

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>后续步骤

可验证网关连接。 请参阅[验证网关连接](vpn-gateway-verify-connection-resource-manager.md)。
<!--Update_Description: add selector-->