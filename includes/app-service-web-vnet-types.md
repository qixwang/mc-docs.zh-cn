---
author: ccompy
ms.service: app-service-web
ms.topic: include
origin.date: 02/27/2020
ms.date: 03/16/2020
ms.author: v-tawe
ms.openlocfilehash: 979085d6b9821fdd402fb5edd81179cf34b5a357
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79546872"
---
* 支持除独立定价计划以外的全部定价计划的多租户系统
* 部署到 VNet 中且支持独立定价计划应用的应用服务环境 (ASE)

本文档介绍 VNet 集成功能，该功能适合在多租户应用中使用。 如果应用在[应用服务环境][ASEintro]中，则该应用已处于 VNet 中且不需要使用 VNet 集成功能来获取同一 VNet 中的资源。 有关所有网络功能的详细信息，请阅读[应用服务网络功能][Networkingfeatures]

VNet 集成允许应用访问虚拟网络中的资源，但不允许通过 VNet 对应用进行入站专用访问。 专用站点访问指的是仅可从专用网络（例如 Azure 虚拟网络内）对应用进行访问。 VNet 集成仅用于从应用对 VNet 进行出站调用。VNet 集成功能有以下变化形式。

<!-- * Regional VNet Integration - When connecting to Resource Manager VNets in the same region, you must have a dedicated subnet in the VNet you are integrating with. -->

* 需要网关的 VNet 集成 - 连接到其他区域中的 VNet 或者连接到同一区域中的经典 VNet 时，需要在目标 VNet 中预配虚拟网络网关。

VNet 集成功能：

* 需要“标准”、“高级”、“高级 V2”或“弹性高级”定价计划 
* 支持 TCP 和 UDP
* 适用于应用服务应用和函数应用

VNet 集成不支持某些功能，其中包括：

* 装载驱动器
* AD 集成 
* NetBios

需要网关的 VNet 集成仅提供对目标 VNet 中的资源，或者通过对等互连或 VPN 连接到目标 VNet 的网络中的资源的访问。 需要网关的 VNet 集成不支持访问可通过 ExpressRoute 连接使用的资源，也不适用于服务终结点。 

VNet 集成功能允许应用访问虚拟网络中的资源，但不允许通过虚拟网络对应用进行入站专用访问。 专用站点访问指的是仅可从专用网络（例如 Azure 虚拟网络内）对应用进行访问。 VNet 集成只是为了从应用对 VNet 进行出站调用。 

<!--Links-->
[ASEintro]: https://docs.azure.cn/app-service/environment/intro
[Networkingfeatures]: https://docs.azure.cn/app-service/networking-features