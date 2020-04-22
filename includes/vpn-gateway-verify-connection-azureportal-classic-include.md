---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 10/17/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: df4c7a5e12072725575752ad1ddea51156df7d45
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63826649"
---
在 Azure 门户中，可通过导航到连接来查看经典 VNet VPN 网关的连接状态。 以下步骤演示导航到连接并进行验证的一种方法。

1. 在 [Azure 门户](http://portal.azure.cn)中单击“所有资源”，然后导航到经典虚拟网络。 
2. 在虚拟网络边栏选项卡中，单击“概述”访问该边栏选项卡的“VPN 连接”部分。  
3. 在 VPN 连接图中单击站点。

    ![本地站点](./media/vpn-gateway-verify-connection-azureportal-classic/localsitename.png "本地站点")
4. 在“站点到站点 VPN 连接”边栏选项卡中，查看有关站点的信息。 

    ![连接状态](./media/vpn-gateway-verify-connection-azureportal-classic/siteconnectstatus.png "连接状态")
5. 若要查看有关连接的详细信息，请单击连接名称，打开“站点到站点 VPN 连接”  边栏选项卡。

    ![连接状态 - 更多](./media/vpn-gateway-verify-connection-azureportal-classic/connections4.png "连接状态 - 更多信息")