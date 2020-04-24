---
title: include 文件
description: include 文件
services: virtual-machines-windows
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: include
origin.date: 09/12/2018
ms.date: 11/12/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 36922bffdd5f4d3b47c7b63df65cb2d6db4b162b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63845703"
---
通过在子网或 VM 网络接口上创建网络筛选器可为 Azure 中的虚拟机 (VM) 打开端口或创建终结点。 将这些筛选器（控制入站和出站流量）置于附加到接收流量的资源的网络安全组中。

本文中的示例演示了如何创建使用标准 TCP 端口 80 的网络筛选器（假设已启动了相应的服务并在 VM 上打开了任何 OS 防火墙规则）。

在创建配置为在标准 TCP 端口 80 上处理 Web 请求的 VM 之后，可以：

1. 创建网络安全组。

2. 创建允许流量的入站安全规则并将值分配给以下设置：

   - **目标端口范围**：80

   - **源端口范围**：*（允许任何源端口）

   - **优先级值**：输入优先级小于 65,500 且高于默认 catch-all 拒绝入站规则的值。

3. 将网络安全组与 VM 网络接口或子网相关联。

    虽然此示例使用简单规则来允许 HTTP 流量，但你也可以使用网络安全组和规则来创建更复杂的网络配置。