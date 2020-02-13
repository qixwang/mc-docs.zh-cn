---
title: 有关 Azure 网络观察程序的常见问题解答 (FAQ) | Microsoft Docs
description: 本文解答有关 Azure 网络观察程序服务的常见问题。
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2019
ms.author: damendo
ms.openlocfilehash: 39068b67bfefd03aa0adaec7a67e714dda7ce35d
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77029368"
---
# <a name="frequently-asked-questions-faq-about-azure-network-watcher"></a>有关 Azure 网络观察程序的常见问题解答 (FAQ)
[Azure 网络观察程序](/network-watcher/network-watcher-monitoring-overview)服务提供一套工具用于监视、诊断 Azure 虚拟网络中的资源、查看其指标，以及为其启用或禁用日志。 本文解答有关该服务的常见问题。

## <a name="general"></a>常规

### <a name="what-is-network-watcher"></a>什么是网络观察程序？
网络观察程序用于监视和修复 IaaS（基础结构即服务）组件的网络运行状况，其中包括虚拟机、虚拟网络、应用程序网关、负载均衡器，以及 Azure 网络中的其他资源。 它不是用于监视 PaaS（平台即服务）基础结构或获取 Web/移动分析数据的解决方案。

### <a name="what-tools-does-network-watcher-provide"></a>网络观察程序提供哪些工具？
网络观察程序提供三个主要功能集
* 监视
  * [拓扑视图](/network-watcher/view-network-topology)显示虚拟网络中的资源及其相互关系。
  * [连接监视器](/network-watcher/connection-monitor)可用于监视 VM 与另一网络资源之间的连接和延迟。
  * [网络性能监视器](/azure-monitor/insights/network-performance-monitor)可用于监视混合网络体系结构、Expressroute 线路和服务/应用程序终结点之间的连接和延迟。  
* 诊断
  * [IP 流验证](/network-watcher/network-watcher-ip-flow-verify-overview)可用于在 VM 级别检测流量筛选问题。
  * [下一跃点](/network-watcher/network-watcher-next-hop-overview)可帮助验证流量路由和检测路由问题。
  * [连接故障排除](/network-watcher/network-watcher-connectivity-portal)可以在 VM 与另一网络资源之间实现一次性连接和延迟检查。
  * [数据包捕获](/network-watcher/network-watcher-packet-capture-overview)可用于捕获虚拟网络中 VM 上的所有流量。
  * [VPN 故障排除](/network-watcher/network-watcher-troubleshoot-overview)针对 VPN 网关和连接运行多项诊断检查，以帮助调试问题。
* 日志记录
  * [NSG 流日志](/network-watcher/network-watcher-nsg-flow-logging-overview)可用于在[网络安全组 (NSG)](/virtual-network/security-overview) 中记录所有流量
  * [流量分析](/network-watcher/traffic-analytics)处理 NSG 流日志数据，使你能够可视化、查询、分析和了解网络流量。


有关更多详细信息，请参阅[网络观察程序概述页](/network-watcher/network-watcher-monitoring-overview)。


### <a name="how-does-network-watcher-pricing-work"></a>网络观察程序如何定价？
有关网络观察程序组件及其定价，请访问[定价页](https://www.azure.cn/zh-cn/pricing/details/network-watcher/)。

### <a name="which-regions-is-network-watcher-available-in"></a>网络观察程序可在哪些区域使用？
可以在 [Azure 服务可用性](https://azure.microsoft.com/global-infrastructure/services/?products=network-watcher)页上查看最新的区域可用性

### <a name="what-are-resource-limits-on-network-watcher"></a>网络观察程序的资源限制是怎样的？
有关所有限制，请参阅[服务限制](/azure-subscription-service-limits#network-watcher-limits)页。  

### <a name="why-is-only-one-instance-of-network-watcher-allowed-per-region"></a>为何每个区域只允许一个网络观察程序实例？
仅需为订阅启用网络观察程序一次，即可正常使用其功能，这并不是一项服务限制。

## <a name="nsg-flow-logs"></a>NSG 流日志

### <a name="what-does-nsg-flow-logs-do"></a>NSG 流日志有什么作用？
可以通过[网络安全组 (NSG)](/virtual-network/security-overview) 来合并和管理 Azure 网络资源。 使用 NSG 流日志可以通过 NSG 记录有关所有流量的 5 元组流信息。 原始流日志将写入 Azure 存储帐户，在存储帐户中，可以根据需要进一步处理、分析、查询或导出这些日志。

### <a name="are-there-any-caveats-to-using-nsg-flow-logs"></a>使用 NSG 流日志时是否有任何注意事项？
无需满足任何先决条件即可使用 NSG 流日志。 但存在两项限制
- **VNET 中不得存在服务终结点**：NSG 流日志从 VM 上的代理发送到存储帐户。 但是，目前只能将日志直接发送到存储帐户，而不能使用 VNET 中添加的服务终结点。

- **不得将存储帐户加入防火墙**：由于内部限制，必须可以通过公共 Internet 访问存储帐户，这样，NSG 流日志才能使用存储帐户。 流量在内部仍然通过 Azure 路由，并且不会产生额外的传出费用。

有关如何解决这些问题的说明，请参阅接下来的两个问题。 这两项限制预计会在 2020 年 1 月得到解决。

### <a name="how-do-i-use-nsg-flow-logs-with-service-endpoints"></a>如何将 NSG 流日志与服务终结点配合使用？

*选项 1：重新配置 NSG 流日志，在没有 VNET 终结点的情况下将其发送到 Azure 存储帐户*

* 查找包含终结点的子网：

    - 在 Azure 门户顶部的全局搜索框中搜索“资源组” 
    - 导航到包含要使用的 NSG 的资源组
    - 使用第二个下拉列表按类型进行筛选，然后选择“虚拟网络” 
    - 单击包含服务终结点的虚拟网络
    - 在左窗格中的“设置”下，选择“服务终结点”  
    - 记下在其中启用了 **Microsoft.Storage** 的子网

* 禁用服务终结点：

    - 继续上面的操作，在左窗格中的“设置”下选择“子网”  
    * 单击包含服务终结点的子网
    - 在“服务终结点”  部分的“服务”下面，取消选中“Microsoft.Storage”  

可以在数分钟后检查存储日志，应该会看到时间戳已更新，或者会看到新的 JSON 文件已创建。

*选项 2：禁用 NSG 流日志*

如果 Microsoft.Storage 服务终结点是必需项，则需禁用 NSG 流日志。

### <a name="how-do-i-disable-the--firewall-on-my-storage-account"></a>如何在存储帐户上禁用防火墙？

启用“所有网络”来访问存储帐户即可解决此问题：

* 在 [NSG 流日志概览页](https://ms.portal.azure.cn/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs)上找到 NSG 即可找到存储帐户的名称
* 在门户的全局搜索框中键入存储帐户的名称，导航到存储帐户
* 在“设置”部分，选择“防火墙和虚拟网络”  
* 选择“所有网络”，然后进行保存。  如果已选中，则不需进行更改。  

### <a name="what-is-the-difference-between-flow-logs-versions-1--2"></a>流日志版本 1 和 2 有何区别？
流日志版本 2 引入了“流状态”的概念，并会存储有关传输的字节和数据包的信息。  [了解详细信息](/network-watcher/network-watcher-nsg-flow-logging-overview#log-file)。

## <a name="next-steps"></a>后续步骤
 - 访问我们的[文档概述页](/network-watcher/)，其中提供了一些网络观察程序入门教程。
