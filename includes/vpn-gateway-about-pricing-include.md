---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 01/21/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: b958e2ae67ce901b17a15c9a83b18bb923f1d9c7
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63844498"
---
支付两项内容：虚拟网络网关的每小时计算成本和虚拟网络网关的出口数据传输。 可在 [定价](https://www.azure.cn/pricing/details/vpn-gateway) 页上找到定价信息。

**虚拟网络网关计算成本**<br>每个虚拟网络网关都有每小时计算成本。 价格基于创建虚拟网络网关时指定的网关 SKU。 成本与网关本身以及流经网关的数据传输相关。 主动-主动设置的成本与主动-被动设置的成本相同。

**数据传输成本**<br>数据传输成本根据源虚拟网络网关的出口流量计算。

* 如果要将流量发送到本地 VPN 设备，以 Internet 出口数据传输率收取费用。
* 如果要在不同区域的虚拟网络之间发送流量，定价因区域而异。
* 如果要仅在属于同一区域的虚拟网络之间发送流量，则没有数据成本。 同一区域的 VNet 之间的流量免费。