---
title: include 文件
description: include 文件
services: load balancer
author: WenJason
ms.service: load-balancer
ms.topic: include
origin.date: 02/08/2018
ms.date: 05/11/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 775a353bce78653b51c05b0d43276959a7e1be53
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82507664"
---
| | 标准负载均衡器 | 基本负载均衡器 |
| --- | --- | --- |
| [后端池大小](/azure-resource-manager/management/azure-subscription-service-limits#load-balancer) | 最多支持 1000 个实例。 | 最多支持 300 个实例。 |
| 后端池终结点 | 单个虚拟网络中的任何虚拟机或虚拟机规模集。 | 单个可用性集或虚拟机规模集中的虚拟机。 |
| [运行状况探测](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP、HTTP、HTTPS | TCP、HTTP |
| [运行状况探测停止行为](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP 连接在实例探测停止时以及在所有探测停止时保持活动状态  。 | TCP 连接在实例探测停止时保持活动状态。 所有 TCP 连接在所有探测停止时都会终止。 |
| 诊断 | [Azure Monitor 多维指标](../articles/load-balancer/load-balancer-standard-diagnostics.md) | 不可用。 |
| HA 端口 | [可用于内部负载均衡器](../articles/load-balancer/load-balancer-ha-ports-overview.md) | 不可用 |
| 默认保护 | 除非网络安全组允许，否则对入站流关闭。 请注意，允许从 VNet 到内部负载均衡器的内部流量。 | 默认处于打开状态。 可选的网络安全组。 |
| 出站规则 | [声明性出站 NAT 配置](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | 不可用 |
| 空闲时 TCP 重置 | [可用于任何规则](../articles/load-balancer/load-balancer-tcp-reset.md) | 不可用 |
| [多个前端](../articles/load-balancer/load-balancer-multivip-overview.md) | 入站和[出站](../articles/load-balancer/load-balancer-outbound-connections.md) | 仅限入站 |
| 管理操作 | 大多数操作都小于 30 秒 | 通常为 60 - 90 多秒 |
| SLA | [99.99%](https://www.azure.cn/support/sla/load-balancer/index.html) | 不可用 | 
