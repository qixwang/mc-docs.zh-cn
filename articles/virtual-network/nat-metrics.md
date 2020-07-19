---
title: Azure 虚拟网络 NAT 的指标和警报
titleSuffix: Azure Virtual Network
description: 了解 Azure 虚拟网络 NAT 可用的 Azure Monitor 指标和警报。
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
ms.service: virtual-network
ms.subservice: nat
Customer intent: As an IT administrator, I want to understand available Azure Monitor metrics and alerts for Virtual Network NAT.
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 03/04/2020
ms.date: 07/13/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 1e39006cc33eb532d8734e1ce7cd2c9449cb29b9
ms.sourcegitcommit: 2bd0be625b21c1422c65f20658fe9f9277f4fd7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2020
ms.locfileid: "86441211"
---
<!--Verified successfully-->
# <a name="azure-virtual-network-nat-metrics"></a>Azure 虚拟网络 NAT 指标

Azure 虚拟网络 NAT 网关资源提供多维指标。 可以使用这些指标来观察操作以及进行[故障排除](troubleshoot-nat.md)。  可以针对严重问题（例如 SNAT 耗尽）配置警报。

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="用于出站 Internet 连接的虚拟网络 NAT">
</p>

图：*用于出站 Internet 连接的虚拟网络 NAT*

## <a name="metrics"></a>指标

NAT 网关资源在 Azure Monitor 中提供以下多维指标：

| 指标 | 说明 | 建议的聚合 | 维度 |
|---|---|---|---|
| 字节 | 处理的入站和出站字节数 | SUM | 方向（传入；传出），协议（6 TCP；17 UDP） |
| 数据包数 | 处理的入站和出站数据包数 | SUM | 方向（传入；传出），协议（6 TCP；17 UDP） |
| 丢弃的数据包数 | NAT 网关丢弃的数据包数 | SUM | / |
| SNAT 连接计数 | 每个间隔的状态转换次数 | SUM | 连接状态，协议（6 TCP；17 UDP） |
| SNAT 连接总数 | 当前的活动 SNAT 连接数（使用的大约 SNAT 端口数） | SUM | 协议（6 TCP；17 UDP） |

## <a name="alerts"></a>警报

可以在 Azure Monitor 中针对上述每个[指标](#metrics)配置警报。

## <a name="limitations"></a>限制

不支持资源运行状况。

## <a name="next-steps"></a>后续步骤

* 了解[虚拟网络 NAT](nat-overview.md)。
* 了解 [NAT 网关资源](nat-gateway-resource.md)
* 了解 [Azure Monitor](../azure-monitor/overview.md)
* 了解如何[排查 NAT 网关资源问题](troubleshoot-nat.md)。
* [在 UserVoice 中告诉我们下一步为虚拟网络 NAT 构建什么](https://aka.ms/natuservoice)。

<!-- Update_Description: new article about nat metrics -->
<!--NEW.date: 07/13/2020-->