---
title: 用于配置 Azure ExpressRoute 线路的工作流
description: 本页指导完成配置 ExpressRoute 线路和对等互连的工作流
services: expressroute
author: cherylmc
ms.service: expressroute
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 08/29/2018
ms.author: v-yiso
ms.date: 10/15/2018
ms.openlocfilehash: dfdd08dbc6db4b9d98df9b40c3865a37624c725f
ms.sourcegitcommit: 091c672fa448b556f4c2c3979e006102d423e9d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87162325"
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>ExpressRoute 线路预配工作流和线路状态
本页从较高层面引导完成服务预配和路由配置工作流。 

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

下图和相应的步骤概述了用于端到端预配 ExpressRoute 线路的任务。 

1. 使用 PowerShell 配置 ExpressRoute 线路。 有关更多详细信息，请遵循[创建 ExpressRoute 线路](./expressroute-howto-circuit-classic.md)一文中的说明。

2. 从服务提供商订购连接。 此过程根据情况而有所不同。 有关如何订购连接的详细信息，请联系连接服务提供商。

3. 通过 PowerShell 验证 ExpressRoute 线路预配状态，以确保线路预配成功。 
4. 配置路由域 如果连接服务提供商管理第 3 层配置，他们将为你的线路配置路由。 如果连接服务提供商只提供第 2 层服务，必须根据[路由要求](expressroute-routing.md)和[路由配置](expressroute-howto-routing-classic.md)页中所述的每条指导原则来配置路由。
   
   * 启用 Azure 专用对等互连 - 启用此对等互连以连接到部署在虚拟网络中的 VM/云服务。
     >[!IMPORTANT]
     > 必须确保使用独立的代理/边缘，而不是用于 Internet 的 代理/边缘来连接 Microsoft。 对 ExpressRoute 和 Internet 使用相同的边缘会导致路由不对称，并造成网络连接中断。
     > 
     > 
     ![](./media/expressroute-workflows/routing-workflow.png)

5. 将虚拟网络链接到 ExpressRoute 线路 - 可以将虚拟网络链接到 ExpressRoute 线路。 请按照说明[将 VNet 链接](./expressroute-howto-linkvnet-arm.md)到用户的线路。 这些 VNet 可以位于 ExpressRoute 线路所在的同一 Azure 订阅中，也可以位于不同的订阅中。

## <a name="expressroute-circuit-provisioning-states"></a>ExpressRoute 线路预配状态

每条 ExpressRoute 线路有两种状态：

- 服务提供商预配状态
- 状态

“状态”表示 Microsoft 的预配状态。 创建 Expressroute 线路时，此属性设置为 Enabled

连接服务提供商预配状态表示连接服务提供商一端的状态。 该状态可能是 NotProvisioned  、Provisioning  或 Provisioned  。 ExpressRoute 线路必须处于 Provisioned 状态，才能配置对等互联。

### <a name="possible-states-of-an-expressroute-circuit"></a>ExpressRoute 线路的可能状态
本部分列出 ExpressRoute 线路的可能状态。

**创建时**

ExpressRoute 线路会在资源创建时报告以下状态。

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**连接服务提供商正在预配线路时**

连接服务提供商预配线路时，ExpressRoute 线路将报告以下状态。

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**连接服务提供商完成预配过程时**

连接服务提供商成功预配线路后，ExpressRoute 线路将报告以下状态。

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


**连接服务提供商正在取消预配线路时**

如果需要取消预配 ExpressRoute 线路，在服务提供商完成取消预配过程后，线路将报告以下状态。

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


如果需要，可以选择重新启用线路，或运行 PowerShell cmdlet 删除线路。  

> [!IMPORTANT]
> ServiceProviderProvisioningState 为 Provisioning 或 Provisioned 时，无法删除线路。 连接服务提供商需要取消预配线路，然后才能将其删除。 在 Azure 中删除 ExpressRoute 线路资源之前，Microsoft 将继续对线路进行计费。
> 

## <a name="routing-session-configuration-state"></a>路由会话配置状态
BGP 预配状态会报告是否已在 Microsoft Edge 上启用 BGP 会话。 必须启用该状态才能使用专用对等互连或 Microsoft 对等互连。

必须特意检查 Microsoft 对等互连的 BGP 会话状态。 除了 BGP 预配状态，还有另一个状态称为 *播发的公共前缀状态*。 播发的公共前缀状态必须是“已配置”状态，BGP 会话才能启动，才能进行端到端的路由。 

如果播发的公共前缀状态设置为需要验证状态，则不会启用 BGP 会话，因为播发的前缀不符合任何路由注册表中的 AS 编号。 

>[!IMPORTANT]
> 如果播发的公共前缀状态是“手动验证”状态，则需要向 [Microsoft 支持](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)开具支持票证，并提供拥有播发 IP 地址的证明以及相关的自治系统编号。
> 
> 

## <a name="next-steps"></a>后续步骤

- 配置 ExpressRoute 连接。

    - [创建 ExpressRoute 线路](./expressroute-howto-circuit-arm.md)
    - [配置路由](./expressroute-howto-routing-arm.md)
    - [将 VNet 链接到 ExpressRoute 线路](./expressroute-howto-linkvnet-arm.md)