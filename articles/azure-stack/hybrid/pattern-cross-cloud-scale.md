---
title: 在 Azure 和 Azure Stack Hub 上生成可跨云缩放的应用程序的模式。
description: 了解如何使用 Azure 和 Azure Stack Hub 生成可跨云缩放的应用。
author: WenJason
ms.service: azure-stack
ms.topic: article
origin.date: 11/05/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: 971f152ca482985e1eb6f1ed27890fa6e938eafe
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74020559"
---
# <a name="cross-cloud-scaling-pattern"></a>跨云缩放模式

自动将资源添加到现有的应用程序，以适应负载的增长。

## <a name="context-and-problem"></a>上下文和问题

应用无法提高容量来满足意外增长的需求。 缺少可伸缩性会导致用户在高峰期无法访问应用。 应用可为固定数目的用户提供服务。

全球企业需要安全、可靠且可用的基于云的应用。 满足增长的需求，并使用适当的基础结构来支持此需求至关重要。 为了在成本和维护以及业务数据安全、存储和实时可用性之间取得平衡，企业费尽了心力。

你可能无法在公有云中运行应用程序。 但是，要让企业在本地环境中保持用于处理应用需求高峰的容量，在经济上似乎不切实际。 通过此模式，可对本地解决方案利用公有云的弹性。

## <a name="solution"></a>解决方案

跨云缩放模式使用公有云资源扩展本地云中的应用。 该模式在需求增大或减小时触发，并相应地在云中添加或删除资源。 这些资源提供冗余、快速可用性和地域合规的路由。

![跨云缩放模式](media/pattern-cross-cloud-scale/cross-cloud-scaling.png)

> [!NOTE]
> 此模式仅适用于应用的无状态组件。

## <a name="components"></a>组件

跨云缩放模式由以下组件构成。

**流量管理器**  

在图中，流量管理器位于公有云组的外部，但它需要能够在本地数据中心和公有云中协调流量。 均衡器通过监视终结点并在必要时提供故障转移再分发，来提供应用程序的高可用性。

**域名系统管理 (DNS)**  

域名系统或 DNS 负责将网站或服务名称转换（或解析）为它的 IP 地址。

### <a name="cloud"></a>云

**托管的生成服务器**  
用于托管生成管道的环境。

**应用程序资源**  
应用程序资源（例如 VM 规模集和容器）需要能够横向缩减和扩展。

**自定义域名**  
使用自定义域名来路由请求 glob。

**公共 IP 地址**  
公共 IP 地址用于将传入流量通过流量管理器路由到公有云应用程序资源终结点。  

### <a name="local-cloud"></a>本地云

**托管的生成服务器**  
用于托管生成管道的环境。

**应用程序资源**  
应用程序资源（例如 VM 规模集和容器）需要能够横向缩减和扩展。

**自定义域名**  
使用自定义域名来路由请求 glob。

**公共 IP 地址**  
公共 IP 地址用于将传入流量通过流量管理器路由到公有云应用程序资源终结点。 

## <a name="issues-and-considerations"></a>问题和注意事项

在决定如何实现此模式时，请考虑以下几点：

### <a name="scalability"></a>可伸缩性

跨云缩放的关键组件提供按需缩放能力。 缩放必须在公有云和本地云基础结构之间进行，并根据需求提供一致且可靠的服务。

### <a name="availability"></a>可用性

确定通过本地硬件配置和软件部署来配置本地部署的应用，以实现高可用性。

### <a name="manageability"></a>可管理性

跨云模式确保在环境之间提供无缝的管理和熟悉的界面。

## <a name="when-to-use-this-pattern"></a>何时使用此模式

使用此模式：

- 需要提高应用程序容量来应对意外的需求或定期需求时。
- 不想要投资购买只有高峰期才用到的资源， 只想为所用的资源付费时。

对于以下情况不建议使用此模式：

- 解决方案要求用户通过 Internet 进行连接。
- 企业所在地的法规要求发起的连接来自现场。
- 网络遇到使缩放性能受限的一般瓶颈。
- 环境已从 Internet 断开连接，因此无法访问公有云。

## <a name="next-steps"></a>后续步骤

若要详细了解本文中介绍的主题：
- 请参阅 [Azure 流量管理器概述](/traffic-manager/traffic-manager-overview)，详细了解这个基于 DNS 的流量负载均衡器的工作原理。
- 请参阅[混合应用程序设计注意事项](overview-app-design-considerations.md)，其中详细介绍了最佳做法，并解答了其他问题。
- 请参阅 [Azure Stack 产品和解决方案系列](/azure-stack)详细了解产品和解决方案的整个阵容。

准备好测试解决方案示例时，请继续阅读[跨云缩放解决方案部署指南](solution-deployment-guide-cross-cloud-scaling.md)。 该部署指南逐步说明了如何部署和测试 Azure Stack 的组件。 其中还介绍了如何创建可提供手动触发过程的跨云解决方案，以便从 Azure Stack Hub 托管的 Web 应用切换到 Azure 托管的 Web 应用。 此外，介绍了如何通过流量管理器使用自动缩放功能，来确保在承受负载的情况下保持云设施的灵活性和可伸缩性。
