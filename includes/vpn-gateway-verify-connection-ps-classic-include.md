---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 10/17/2018
ms.date: 12/24/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9e4e3d92ad096bcc1cae6c5a89ea695b358ffb96
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63844700"
---
可使用“Get-AzureVNetConnection”cmdlet 来验证连接是否已成功。

1. 使用以下 cmdlet 示例，配置符合自己需要的值。 如果虚拟网络的名称包含空格，必须将该名称括在引号中。

   ```azurepowershell
   Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
   ```
2. cmdlet 运行完毕后，查看该值。 在以下示例中，“连接状态”显示为“已连接”，且可以看到入口和出口字节数。

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal