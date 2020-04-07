---
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 11/09/2018
ms.date: 04/06/2020
ms.author: v-jay
ms.openlocfilehash: 4bc0dc626df28e996936840ea8bf7bcdd24d3447
ms.sourcegitcommit: 5fb45da006859215edc8211481f13174aa43dbeb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80634550"
---
传统（旧版）VPN 网关 SKU 为：

* 默认值（基本）
* 标准
* HighPerformance

VPN 网关不使用 UltraPerformance 网关 SKU。 有关 UltraPerformance SKU 的信息，请参阅 [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) 文档。

使用传统 SKU 时，请考虑以下方面：

* 如果想要使用 PolicyBased VPN 类型，必须使用基本 SKU。 任何其他 SKU 均不支持 PolicyBased VPN（之前称为静态路由）。
* 基本 SKU 不支持 BGP。
* 基本 SKU 不支持 ExpressRoute-VPN 网关共存配置。
* 主动-主动 S2S VPN 网关连接只能在 HighPerformance SKU 上配置。