---
title: 常见问题解答 - Azure 负载均衡器
description: 有关 Azure 负载均衡器的常见问题解答。
services: load-balancer
author: WenJason
ms.service: load-balancer
ms.topic: article
origin.date: 04/22/2020
ms.date: 06/08/2020
ms.author: v-jay
ms.openlocfilehash: 01a86e77c447851a58b9646999ba7ca875dd7e2c
ms.sourcegitcommit: 9811bf312e0d037cb530eb16c8d85238fd276949
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84275671"
---
# <a name="frequently-asked-questions"></a>常见问题

## <a name="what-types-of-load-balancer-exist"></a>存在哪些类型的负载均衡器？
内部负载均衡器（用于均衡 VNET 中的流量）和外部负载均衡器（用于均衡进出 Internet 连接的终结点的流量）。 有关详细信息，请参阅[负载均衡器类型](components.md#frontend-ip-configurations)。 

对于这两种类型，Azure 提供了基本 SKU 和标准 SKU，它们具有不同的功能、性能、安全性和运行状况跟踪功能。 [SKU 比较](skus.md)一文中介绍了这些差异。

 ## <a name="how-can-i-upgrade-from-a-basic-to-a-standard-load-balancer"></a>如何从基本负载均衡器升级到标准负载均衡器？
有关自动化脚本和升级负载均衡器 SKU 的指南，请参阅[从“基本”升级到“标准”](upgrade-basic-standard.md)一文。

 ## <a name="what-are-the-different-load-balancing-options-in-azure"></a>Azure 中有哪些不同的负载均衡选项？
请参阅[负载均衡器技术指南](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview)，了解可用的负载均衡服务以及每个服务的建议用途。

## <a name="where-can-i-find-load-balancer-arm-templates"></a>在哪里可以找到负载均衡器 ARM 模板？
有关常见部署的 ARM 模板，请参阅 [Azure 负载均衡器快速入门模板列表](https://docs.microsoft.com/azure/templates/microsoft.network/loadbalancers#quickstart-templates)。

## <a name="how-are-inbound-nat-rules-different-from-load-balancing-rules"></a>入站 NAT 规则与负载均衡规则有何不同？
NAT 规则用于指定要将流量路由到其中的后端资源。 例如，将特定负载均衡器端口配置为将 RDP 流量发送到特定 VM。 负载均衡规则用于指定要将流量路由到其中的后端资源池，从而在每个实例之间均衡负载。 例如，负载均衡器规则可以跨 Web 服务器池在负载均衡器的端口 80 上路由 TCP 数据包。

