---
title: Azure 网络观察程序中的“下一跃点”简介 | Azure
description: 本文概述了网络观察程序的“下一个跃点”功能。
services: network-watcher
documentationcenter: na
author: rockboyfor
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 02/22/2017
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: d2beae111c95c7d56179634ef1d842a962a4b80b
ms.sourcegitcommit: 3eadca6821ef679d8ac6ca2dc46d6a13aac211cd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87548046"
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>使用“下一个跃点”功能来诊断虚拟机路由问题

来自虚拟机 (VM) 的流量将基于与网络接口 (NIC) 关联的有效路由发送到目标。 “下一个跃点”功能可获取来自特定 VM 和 NIC 的数据包的下一跃点类型和 IP 地址。 了解下一个跃点可帮助你确定流量是否定向到预定目标，或流量是否无处发送。 不正确的路由配置（将流量定向到本地位置或虚拟设备）可能会导致连接问题。 “下一跃点”功能也返回与下一跃点关联的路由表。 如果路由定义为用户定义的路由，则将返回该路由。 否则，“下一个跃点”功能返回“系统路由”  。

:::image type="content" source="./media/network-watcher-next-hop-overview/figure1.png" alt-text="下一跃点概述":::

由“下一个跃点”功能返回的下一个跃点如下所示：

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VirtualNetwork
* VirtualNetworkPeering
* 虚拟网络服务终结点 
* MicrosoftEdge
* 无

要详细了解每种下一个跃点类型，请参阅[路由概述](../virtual-network/virtual-networks-udr-overview.md?toc=%2fnetwork-watcher%2ftoc.json)。

## <a name="next-steps"></a>后续步骤

要了解如何使用“下一个跃点”功能来诊断 VM 网络路由问题，请参阅“使用 [Azure 门户](diagnose-vm-network-routing-problem.md)、[PowerShell](diagnose-vm-network-routing-problem-powershell.md) 或 [Azure CLI](diagnose-vm-network-routing-problem-cli.md) 诊断 VM 网络路由问题”。

<!-- Update_Description: update meta properties, wording update, update link -->