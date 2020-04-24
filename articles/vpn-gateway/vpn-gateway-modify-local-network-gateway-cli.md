---
title: 修改本地网络网关 IP 地址前缀和 VPN 网关 IP 地址 | Azure| CLI | Microsoft Docs
description: 本文介绍了如何使用 Azure CLI 更改本地网络网关的 IP 地址前缀。
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
origin.date: 11/29/2017
ms.date: 04/01/2019
ms.author: v-jay
ms.openlocfilehash: 1c0d6298d627ac9ffc950a6b8a5c5eee372e50c0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63856057"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>使用 Azure CLI 修改本地网络网关设置

有时，本地网络网关的地址前缀或网关 IP 地址的设置会变更。 本文演示如何修改本地网络网关设置。 还可以选择以下列表中的其他选项，使用另一种方法来修改这些设置：

> [!div class="op_single_selector"]
> * [Azure 门户](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before-you-begin"></a><a name="before"></a>准备工作

安装最新版本的 CLI 命令（2.0 或更高版本）。 有关安装 CLI 命令的信息，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>修改 IP 地址前缀

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>修改网关 IP 地址

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>后续步骤

可验证网关连接。 请参阅[验证网关连接](vpn-gateway-verify-connection-resource-manager.md)。

<!--Update_Description: link update -->
