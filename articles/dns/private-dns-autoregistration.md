---
title: 什么是 Azure DNS 专用区域的自动注册功能
description: Azure DNS 专用区域的自动注册功能概述
services: dns
author: WenJason
ms.service: dns
ms.topic: article
origin.date: 9/24/2019
ms.date: 01/13/2020
ms.author: rohink
ms.openlocfilehash: a9c9dcd2f56088d3d72f1807c28e95b5ce74ba4f
ms.sourcegitcommit: 157df1a5f11473dd3265d71f1543437f50f09adb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75777475"
---
# <a name="what-is-the-autoregistration-feature-of-azure-dns-private-zones"></a>什么是 Azure DNS 专用区域的自动注册功能

Azure DNS 专用区域自动注册功能简化了对虚拟机（部署在虚拟网络中）的 DNS 记录的管理过程。 当将[虚拟网络](./private-dns-virtual-network-links.md)与专用 DNS 区域链接并为所有虚拟机启用自动注册时，将自动在专用 DNS 区域中创建虚拟机（部署在虚拟网络中）的 DNS 记录。 除了前向查找记录（A 记录），该功能还会自动为虚拟机创建反向查找记录（PTR 记录）。
如果将更多虚拟机添加到虚拟网络，则这些虚拟机的 DNS 记录也会在链接的专用 DNS 区域中自动进行创建。

删除虚拟机时，虚拟机的 DNS 记录将自动从专用 DNS 区域中删除。

可以通过在创建虚拟网络链接时选择“启用自动注册”选项来启用自动注册。

![启用自动注册](./media/privatedns-concepts/enable-autoregistration.png)

## <a name="restrictions"></a>限制

* 自动注册仅适用于虚拟机。 对于所有其他资源（如内部负载均衡器等），可以在链接到虚拟网络的专用 DNS 区域中手动创建 DNS 记录。
* 仅为主虚拟机 NIC 自动创建 DNS 记录。 如果虚拟机有多个 NIC，则可以手动为其他网络接口创建 DNS 记录。
* 不支持 IPv6 自动注册（AAAA 记录）。

## <a name="next-steps"></a>后续步骤

* 了解如何使用 [Azure PowerShell](./private-dns-getstarted-powershell.md) 或 [Azure CLI](./private-dns-getstarted-cli.md) 在 Azure DNS 中创建专用区域。

* 了解可以通过 Azure DNS 中的专用区域实现的一些常见[专用区域方案](./private-dns-scenarios.md)。

* 有关 Azure DNS 专用区域的一些常见问题和解答，包括对于某些类型的操作可以期待的特定行为，请参阅[专用 DNS 常见问题解答](./dns-faq-private.md)。
