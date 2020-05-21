---
title: include 文件
description: include 文件
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: include
origin.date: 04/07/2020
ms.date: 05/18/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: c73e1082e46fe7408fe932931bed46dd1242ee7b
ms.sourcegitcommit: 8d56bc6baeb42d675695ecef1909d76f5c4a6ae3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83406212"
---
| 资源 | 限制 |
| --- | --- |
| 数据吞吐量 |30 Gbps<sup>1</sup> |
|规则|10,000。 已组合所有规则类型。|
|最小 AzureFirewallSubnet 大小 |/26|
|网络和应用程序规则的端口范围|1 - 65535|
|公共 IP 地址|最大为 100|
|路由表|默认情况下，AzureFirewallSubnet 使用其 NextHopType 值设置为“Internet”的 0.0.0.0/0 路由。<br /><br />Azure 防火墙必须具有直接的 Internet 连接。 如果 AzureFirewallSubnet 知道通过 BGP 的本地网络的默认路由，则必须将其替代为 0.0.0.0/0 UDR，将 NextHopType 值设置为 Internet 以保持 Internet 直接连接 。 默认情况下，Azure 防火墙不支持强制的安全加密链路连接到本地网络。<br /><br />但是，如果你的配置要求通过强制隧道连接到本地网络，Azure 将基于具体的情况提供支持。 请联系支持人员，以便我们可以查看你的情况。 如果接受，我们将允许你的订阅，并确保保持所需的防火墙 Internet 连接。|

<!--CURRENTLY NAT NOT AVAILABLE ON AZURE CHINA CLOUD-->
<!--MOONCAKE: IP Group not avilabl till on 05/18/2020-->

<sup>1</sup>如果需要增加这些限制，请与 Azure 支持部门联系。

<!-- Update_Description: update meta properties, wording update, update link -->