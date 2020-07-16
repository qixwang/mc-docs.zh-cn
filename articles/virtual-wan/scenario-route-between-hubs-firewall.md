---
title: 方案 - Azure 防火墙 - Interhub 路由
titleSuffix: Azure Virtual WAN
description: 路由方案 - 在具有 Azure 防火墙的多个中心之间路由
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 06/29/2020
ms.date: 07/13/2020
ms.testscope: no
ms.testdate: 07/13/2020
ms.author: v-yeche
ms.openlocfilehash: 976ecd313449d5ed1595d7529609c5f2046c7938
ms.sourcegitcommit: 6c9e5b3292ade56d812e7e214eeb66aeb9b8776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86218863"
---
# <a name="scenario-azure-firewall---interhub-preview"></a>方案：Azure 防火墙 - 中心间（预览版）

当使用虚拟 WAN 虚拟中心路由时，有很多可用方案。 在此方案中，目标是在包含 Azure 防火墙的多个中心之间路由。 有关虚拟中心路由的信息，请参阅[关于虚拟中心路由](about-virtual-hub-routing.md)。

<a name="architecture"></a>
## <a name="scenario-architecture"></a>方案体系结构

在此方案中，采用以下配置：

* 你有多个中心，每个中心都带有 Azure 防火墙。
* 你使用的是安全虚拟中心。
* 已经为专用流量 (VNet)、Internet 和分支流量设置了适当的策略。
* VNet 到 Internet (V2I)、VNet 到分支 (V2B)、分支到 VNet (B2V)，所有这些流量在每个中心中都要通过 Azure 防火墙。

其他注意事项：

* 对于使用 Azure 防火墙的中心间方案，假设分支 VNet 不与单独的路由表相关联。 （例如，“VNET 1”与“路由表 A”相关联，以及“VNET 2”与“路由表 B”相关联   ）。 所有 VNet 都与 Azure 防火墙的路由所属的同一路由表相关联。
* 通过 Azure 防火墙实施安全保护目前仅限于分支往返 VNet 的流量和 Internet 流量 。 当前不支持经过 Azure 防火墙的“分支到分支”流量。

**图 1**

:::image type="content" source="./media/routing-scenarios/between-hubs-firewall/architecture.png" alt-text="体系结构":::

<a name="workflow"></a>
## <a name="scenario-workflow"></a>方案工作流

若要配置此方案，请考虑以下步骤：

<a name="step-1"></a>
### <a name="step-1"></a>步骤 1

假设已通过 Azure 防火墙管理器保护连接，第一步是确保所有分支和 VNet 连接传播“无”。 这是确保将流量设置为经过 Azure 防火墙所必需的。

1. 选择中心并编辑“无”路由表。
1. 更新“传播”以选择所有分支和所有 VNet。

<a name="step-2"></a>
### <a name="step-2"></a>步骤 2

为每个中心设置自定义路由表。

1. 为“中心 1”创建路由表“RT_Hub1” 。

    * 如果使用的是 Azure 防火墙管理器，它会自动为 0.0.0.0/0 在中心的默认路由表中创建一个具有下一个跃点“AZFW1”的路由。 我们将在步骤 3 中修改此设置。 对于“RT_Hub1”，显式地为 0.0.0.0/0 创建一个带有下一个跃点“AZFW1”的条目 。 此设置将通过 AZFW1 激活 V2V、B2V 和 V2I。
    * 由于你希望从“中心 1”通过“AZFW2”（而不是通过 AZFW1）到达“中心 2”分支和 VNet，因此需要添加另外 2 个聚合路由，分别用于“中心 2”分支 (10.5.0.0/16->AZFW2) 和 VNet (10.2.0.0/16->AZFW2)   。

1. 对于“中心 2”，创建路由表“RT_Hub2” 。

    * 如果使用的是 Azure 防火墙管理器，它会自动为 0.0.0.0/0 在中心的默认路由表中创建一个具有下一个跃点“AZFW2”的路由。 我们将在步骤 3 中修改此设置。 对于“RT_Hub2”，使用下一个跃点“AZFW2”显式地为 0.0.0.0/0 创建一个条目 。 此设置通过 AZFW2 激活 V2V、B2V 和 V2I。
    * 由于你希望通过中心 2 的“AZFW2”来保护中心间通信，所以无需显式添加与前一个中心 1 步骤中的第二个项目符号类似的步骤。

<a name="step-3"></a>
### <a name="step-3"></a>步骤 3

修改每个中心的“默认路由表”，以便为经过 Azure 防火墙的“分支到 VNet”(B2V) 和“分支到 Internet”(B2I) 数据流添加一个静态路由  。 分支到分支当前不支持通过 Azure 防火墙。

1. 对于“中心 1 默认路由表”：

    * 分支到中心 2 分支的流量经过 AZFW2：10.5.0.0/16->AZFW2. 此配置为中心 2 防火墙设置路由。
    * 分支到中心 2 Vnet 的流量经过 AZFW2：10.2.0.0/16->AZFW2. 此配置为中心 2 防火墙设置路由。
    * “分支到分支（本地）/分支到 VNet（本地）/分支到 Internet”：0.0.0.0/0->AZFW1.

2. 对于“中心 2 默认路由表”：

    * 分支到分支（本地和远程）/分支到 VNet（本地和远程）/分支到 Internet：0.0.0.0/0->AZFW2.

这将导致路由配置更改，如下图所示

**图 2**

:::image type="content" source="./media/routing-scenarios/between-hubs-firewall/workflow.png" alt-text="workflow":::

## <a name="next-steps"></a>后续步骤

* 有关虚拟 WAN 的详细信息，请参阅[常见问题解答](virtual-wan-faq.md)。
* 有关虚拟中心路由的详细信息，请参阅[关于虚拟中心路由](about-virtual-hub-routing.md)。

<!--Not Available on [How to configure virtual hub routing](how-to-virtual-hub-routing.md)-->

<!-- Update_Description: new article about scenario route between hubs firewall -->
<!--NEW.date: 07/13/2020-->