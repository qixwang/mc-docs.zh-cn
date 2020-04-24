---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 11/04/2019
ms.date: 01/20/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 0575f84501acb25ab6f13da3649225641a6c79bb
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75939766"
---
可以使用 `Resize-AzVirtualNetworkGateway` PowerShell cmdlet 升级或降级第 1 代或第 2 代 SKU（除基本 SKU 外，所有 VpnGw SKU 都可以调整大小）。 如果使用的是基本网关 SKU，[请改用这些说明](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize)来调整网关大小。

以下 PowerShell 示例演示如何将网关 SKU 的大小调整为 VpnGw2。

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

还可以通过转到虚拟网关的“配置”页并从下拉列表中选择其他 SKU，在 Azure 门户中调整网关大小  。
