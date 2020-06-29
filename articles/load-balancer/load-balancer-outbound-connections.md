---
title: Azure 中的出站连接
titleSuffix: Azure Load Balancer
description: 本文介绍 Azure 如何使 VM 与公共 Internet 服务通信。
services: load-balancer
documentationcenter: na
author: WenJason
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 08/07/2019
ms.date: 06/22/2020
ms.author: v-jay
ms.openlocfilehash: 5607974f04d6f64488d49d0692d2aa22471f72df
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85098646"
---
# <a name="outbound-connections-in-azure"></a>Azure 中的出站连接

Azure 负载均衡器通过多种不同的机制为客户部署提供出站连接。 本文介绍具体的方案、其应用方式、工作原理以及管理方式。 如果通过 Azure 负载均衡器进行出站连接时遇到问题，请参阅[出站连接故障排除指南](../load-balancer/troubleshoot-outbound-connection.md)。

>[!NOTE] 
>本文仅涵盖了资源管理器部署。 有关 Azure 中的所有经典部署方案，请查看[出站连接（经典）](load-balancer-outbound-connections-classic.md)。

Azure 中的部署可与 Azure 外部的公共 IP 地址空间中的终结点进行通信。 当实例启动到公共 IP 地址空间中的目标的出站流时，Azure 会动态将专用 IP 地址映射到公共 IP 地址。 创建此映射后，此出站发起流的返回流量还可以抵达发起流的专用 IP 地址。

Azure 使用源网络地址转换 (SNAT) 来执行此功能。 当多个专用 IP 地址伪装成单个公共 IP 地址时，Azure 将使用[端口地址转换 (PAT)](#pat) 来伪装专用 IP 地址。 临时端口用于 PAT，是基于池大小[预先分配](#preallocatedports)的。

有多种[出站方案](#scenarios)。 可根据需要结合这些方案。 请认真分析这些方案，以了解在部署模型和应用方案中应用这些方案时的功能、约束和模式。 查看有关[管理这些方案](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)的指导。

>[!IMPORTANT] 
>标准负载均衡器和标准公共 IP 为出站连接引入了新功能和不同的行为。  它们不同于基本 SKU。  如果在使用标准 SKU 时需要出站连接，则必须使用标准公共 IP 地址或标准公共负载均衡器显式定义它。  这包括在使用内部标准负载均衡器时创建出站连接。  建议始终使用标准公共负载均衡器上的出站规则。  [方案 3](#defaultsnat)不适用于标准 SKU。  这意味着使用内部标准负载均衡器时，如果需要出站连接，则需要采取步骤为后端池中的 VM 创建出站连接。  在出站连接的上下文中，单独的 VM、可用性集中的所有 VM、VMSS 中的所有实例都是一个组。 这意味着，如果可用性集中的单个 VM 与标准 SKU 关联，则该可用性集中的所有 VM 实例现在都遵循相同的规则，就好像这些 VM 实例与标准 SKU 相关联一样，即使单个实例与标准 SKU 没有直接关联。 如果独立 VM 有连接到负载均衡器的多个网络接口卡，也会出现此行为。 如果将一个 NIC 添加为独立 NIC，也会有相同的行为。 请仔细查看整个文档以了解整体概念，查看[标准负载均衡器](load-balancer-standard-overview.md)了解 SKU 之间的差异，并查看[出站规则](load-balancer-outbound-rules-overview.md)。  使用出站规则可以对出站连接的所有方面进行细化管理控制。

## <a name="scenario-overview"></a><a name="scenarios"></a>方案概述

Azure 负载均衡器和相关资源是使用 [Azure 资源管理器](/azure-resource-manager/resource-group-overview)时显式定义的。  Azure 目前提供三种不同的方法实现 Azure 资源管理器资源的出站连接。 

| SKU | 方案 | 方法 | IP 协议 | 说明 |
| --- | --- | --- | --- | --- |
| 标准、基本 | [1.具有实例级公共 IP 地址的 VM（有或没有负载均衡器）](#ilpip) | SNAT，不使用端口伪装 | TCP、UDP、ICMP、ESP | Azure 使用分配实例 NIC 的 IP 配置的公共 IP。 此实例具有所有可用的临时端口。 使用标准负载均衡器时，如果向虚拟机分配了公共 IP，则不支持[出站规则](load-balancer-outbound-rules-overview.md)。 |
| 标准、基本 | [2.与 VM 关联的公共负载均衡器（实例上没有公共 IP 地址）](#lb) | 使用负载均衡器前端进行端口伪装 (PAT) 的 SNAT | TCP、UDP |Azure 与多个专用 IP 地址共享公共负载均衡器前端的公共 IP 地址。 Azure 使用前端的临时端口进行 PAT。 使用标准负载均衡器时，应使用[出站规则](load-balancer-outbound-rules-overview.md)显式定义出站连接。 |
| 无或基本 | [3.独立 VM（无负载均衡器，无公共 IP 地址）](#defaultsnat) | 使用端口伪装 (PAT) 的 SNAT | TCP、UDP | Azure 自动指定用于 SNAT 的公共 IP 地址，与可用性集的多个专用 IP 地址共享此公共 IP 地址，并使用此公共 IP 地址的临时端口。 此方案是前述方案的回退方案。 如果需要可见性和控制，则我们不建议采用。 |

如果不希望 VM 与 Azure 外部的公共 IP 地址空间中的终结点通信，则可以根据需要使用网络安全组 (NSG) 来阻止访问。 [阻止出站连接](#preventoutbound)部分详细介绍了 NSG。 本文不会介绍有关在无任何出站访问权限的情况下，如何设计和管理虚拟网络的设计和实施指导。

### <a name="scenario-1-vm-with-public-ip-address"></a><a name="ilpip"></a>场景 1：使用公共 IP 地址的 VM

在此场景中，向 VM 分配了公共 IP。 就出站连接而言，VM 是否经过负载均衡并不重要。 此方案优先于其他方案。 使用公共 IP 地址时，VM 将该公共 IP 地址用于所有出站流。  

分配到 VM 的公共 IP 属于 1 对 1 关系（而不是 1 对多关系），并实现为无状态的 1 对 1 NAT。  不使用端口伪装 (PAT)，并且 VM 具有所有可供使用的临时端口。

如果应用程序启动很多出站流，并且遇到 SNAT 端口耗尽的情况，可以考虑分配[公共 IP 地址以缓解 SNAT 约束](../load-balancer/troubleshoot-outbound-connection.md#assignilpip)。 请查看[管理 SNAT 耗尽](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)。

### <a name="scenario-2-load-balanced-vm-without-a-public-ip-address"></a><a name="lb"></a>场景 2：无公共 IP 地址的负载均衡 VM

在此方案中，VM 是公共负载均衡器池的一部分。 没有分配给 VM 的公共 IP 地址。 必须为负载均衡器资源配置一个负载均衡器规则，以在公共 IP 前端与后端池之间创建链接。

如果没有完成此规则配置，则行为将如适用于[没有公共 IP 的独立 VM](#defaultsnat) 方案中所述。 不需要在规则中添加后端池的正常运行的侦听器或者运行状况探测就能成功实现。

当负载均衡的 VM 创建出站流时，Azure 将此出站流的专用源 IP 地址转换为公共负载均衡器前端的公共 IP 地址。 Azure 使用 (SNAT) 来执行此功能。 Azure 还使用 [PAT](#pat) 来伪装公共 IP 地址后面的多个专用 IP 地址。 

使用负载均衡器的公共 IP 地址前端的临时端口区分由 VM 产生的各个流。 创建出站流后，SNAT 动态使用[预先分配的临时端口](#preallocatedports)。 在此情况下，用于 SNAT 的临时端口被称为 SNAT 端口。

SNAT 端口是按照[了解 SNAT 和 PAT](#snat) 部分中所述预先分配的。 它们是可能会耗尽的有限资源。 因此了解它们的[使用](#pat)方式很重要。 请查看[管理 SNAT 耗尽](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)，了解如何根据需要进行设计和缓解。

如果[多个公共 IP 地址与负载均衡器基本版相关联](load-balancer-multivip-overview.md)，则所有这些公共 IP 地址都是出站流的候选项，并且会随机选择其中一个。  

若要监视负载均衡器基本版的出站连接运行状况，可以使用[用于负载均衡器的 Azure Monitor 日志](load-balancer-monitor-log.md)和[警报事件日志](load-balancer-monitor-log.md#alert-event-log)来监视 SNAT 端口耗尽消息。

### <a name="scenario-3-standalone-vm-without-a-public-ip-address"></a><a name="defaultsnat"></a>场景 3：无公共 IP 地址的独立 VM

在此场景中，VM 不是公共负载均衡器池的一部分（也不是内部标准负载均衡器池的一部分），并且没有分配给它的公共 IP 地址。 当 VM 创建出站流时，Azure 将此出站流的专用源 IP 地址转换为公共源 IP 地址。 用于此出站流的公共 IP 地址是不可配置的，并且不会影响订阅的公共 IP 资源限制。 此公共 IP 地址不属于你，不能保留。 如果重新部署 VM、可用性集或虚拟机规模集，则将释放此公共 IP 地址并请求新的公共 IP 地址。 请不要使用此方案将 IP 地址加入允许列表。 而是使用其他两个方案之一，其中你显式声明出站方案和要用于出站连接的公共 IP 地址。

>[!IMPORTANT] 
>仅当附加了内部基本负载均衡器时，此场景才适用。 如果已将内部标准负载均衡器附加到 VM，则场景 3 不适用。  除了使用内部标准负载均衡器以外，还必须显式创建[场景 1](#ilpip) 或[场景 2](#lb)。

Azure 结合端口伪装 ([PAT](#pat)) 使用 SNAT 来执行此功能。 此方案类似于[方案 2](#lb)，但无法控制使用的 IP 地址。 这是方案 1 和方案 2 不存在时的回退方案。 如果需要控制出站地址，则我们不建议使用此方案。 如果出站连接是应用程序的关键部分，应该选择另一种方案。

SNAT 端口是根据[了解 SNAT 和 PAT](#snat) 部分中所述预先分配的。  共享可用性集的 VM 数目决定了适用的预分配层。  没有可用性集的独立 VM 实际上是用于确定预分配（1024 SNAT 端口）的、包含 1 个 VM 的池。 SNAT 端口是可能会被耗尽的有限资源。 因此了解它们的[使用](#pat)方式很重要。 请查看[管理 SNAT 耗尽](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)，了解如何根据需要进行设计和缓解。

### <a name="multiple-combined-scenarios"></a><a name="combinations"></a>多个组合方案

可以结合前面部分中所述的方案来实现特定的效果。 存在多个方案时，优先顺序如下：[方案 1](#ilpip) 优先于[方案 2](#lb)和 [3](#defaultsnat)。 [方案 2](#lb) 优先于[方案 3](#defaultsnat)。

例如，在 Azure 资源管理器部署中，应用程序严重依赖于与有限数量的目标建立出站连接，但也通过负载均衡器前端接收入站流。 在此情况下，可以结合方案 1 和 2 来缓解问题。 有关其他模式，请查看[管理 SNAT 耗尽](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)。

### <a name="multiple-frontends-for-outbound-flows"></a><a name="multife"></a>对出站流使用多个前端

#### <a name="standard-load-balancer"></a>标准负载均衡器

存在[多个（公共）IP 前端](load-balancer-multivip-overview.md)时，标准负载均衡器同时使用出站流的所有候选项。 如果对出站连接启用了负载均衡规则，则每个前端的可用预分配 SNAT 端口数将会倍增。

可以使用一个新的负载均衡规则选项，来禁止对出站连接使用某个前端 IP 地址：

```json    
      "loadBalancingRules": [
        {
          "disableOutboundSnat": false
        }
      ]
```

通常，`disableOutboundSnat` 选项默认为 _false_，表示此规则将为负载均衡规则的后端池中关联的 VM 编制出站 SNAT。 可将 `disableOutboundSnat` 更改为 _true_，防止负载均衡器对此负载均衡规则的后端池中 VM 的出站连接使用关联的前端 IP 地址。  此外，仍可以根据[多个组合方案](#combinations)中所述，为出站流指定特定的 IP 地址。

#### <a name="load-balancer-basic"></a>负载均衡器基本版

当[多个（公共）IP 前端](load-balancer-multivip-overview.md)适用于出站流时，负载均衡器基本版将选择单个前端用于出站流。 此项选择不可配置，应将选择算法视为随机。 可以根据[多个组合方案](#combinations)中所述，为出站流指定特定的 IP 地址。

## <a name="understanding-snat-and-pat"></a><a name="snat"></a>了解 SNAT 和 PAT

### <a name="port-masquerading-snat-pat"></a><a name="pat"></a>端口伪装 SNAT (PAT)

当公共负载均衡器资源与没有专用公共 IP 地址的 VM 实例关联时，将重写每个出站连接源。 出站连接源从虚拟网络专用 IP 地址空间重新写入负载均衡器的前端公共 IP 地址。 在公共 IP 地址空间中，流的 5 元组（源 IP 地址、源端口、IP 转换协议、目标 IP 地址、目标端口）必须唯一。 端口伪装 SNAT 可与 TCP 或 UDP IP 协议一起使用。

重写专用源 IP 地址后，临时端口（SNAT 端口）用于实现此目的，因为多个流源自单个公共 IP 地址。 伪装 SNAT 算法的端口为 UDP 与 TCP 分配不同的 SNAT 端口。

#### <a name="tcp-snat-ports"></a><a name="tcp"></a>TCP SNAT 端口

每个到单个目标 IP 地址、端口的流使用一个 SNAT 端口。 对于到相同的目标 IP 地址、端口和协议的多个 TCP 流，每个 TCP 流使用一个 SNAT 端口。 这可以确保源自相同的公共 IP 地址，并到相同的目标 IP 地址、端口和协议的流的唯一性。 

每个流均流到不同目标 IP 地址、端口和协议的多个流共用一个 SNAT 端口。 目标 IP 地址、端口和协议使流保持唯一，无需使用其他源端口来区分公共 IP 地址空间中的流。

#### <a name="udp-snat-ports"></a><a name="udp"></a> UDP SNAT 端口

UDP SNAT 端口由与 TCP SNAT 端口不同的算法管理。  负载均衡器对 UDP 使用称为“端口受限锥形 NAT”的算法。  无论目标 IP 地址、端口如何，每个流都会使用一个 SNAT 端口。

#### <a name="snat-port-reuse"></a>SNAT 端口重用

释放某个端口以后，即可根据需要重复使用该端口。  可以将 SNAT 端口视为一个适用于给定场景的从低到高的序列，第一个可用 SNAT 端口用于新的连接。 
 
#### <a name="exhaustion"></a>耗尽

如果 SNAT 端口资源已经耗尽，那么在现有流释放 SNAT 端口之前出站流会失败。 当流关闭时，负载均衡器将回收 SNAT 端口，并使用 [4 分钟空闲超时](../load-balancer/troubleshoot-outbound-connection.md#idletimeout)回收空闲流中的 SNAT 端口。

由于使用的算法不同，UDP SNAT端口的耗尽速度通常比 TCP SNAT 端口快得多。 在进行设计和规模测试时必须考虑到这种差异。

有关可以使用哪些模式来缓解通常导致 SNAT 端口耗尽的状态，请查看[管理 SNAT](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) 部分。

### <a name="ephemeral-port-preallocation-for-port-masquerading-snat-pat"></a><a name="preallocatedports"></a>端口伪装 SNAT (PAT) 的临时端口预先分配

使用端口伪装 SNAT ([PAT](#pat)) 时，Azure 使用某种算法根据后端池的大小来确定可用的预先分配 SNAT 端口数目。 SNAT 端口是可用于特定公共 IP 源地址的临时端口。 对于与负载均衡器关联的每个公共 IP 地址，有 64,000 个端口可用作每个 IP 传输协议的 SNAT 端口。

将分别为 UDP 和 TCP 预分配相同数量的 SNAT 端口，并根据 IP 传输协议独立地使用这些端口。  但是，SNAT 端口使用情况会因流是 UDP 还是 TCP 而有所不同。

>[!IMPORTANT]
>标准 SKU SNAT 编程依据 IP 传输协议并且派生自负载均衡规则。  如果只存在一个 TCP 负载均衡规则，则 SNAT 仅可用于 TCP。 如果只有一个 TCP 负载均衡规则并且 UDP 需要出站 SNAT，请创建从同一个前端到同一个后端池的 UDP 负载均衡规则。  这将触发针对 UDP 的 SNAT 编程。  不需要采用工作规则或运行状况探测。  无论在负载均衡规则中指定了什么传输协议，基本 SKU SNAT 都始终针对 IP 传输协议编写 SNAT 程序。

Azure 向每个 VM 的 NIC IP 配置预先分配 SNAT 端口。 将 IP 配置添加到池后，将会根据后端池的大小预先分配此 IP 配置的 SNAT 端口。 创建出站流后，当流关闭或[空闲超时](../load-balancer/troubleshoot-outbound-connection.md#idletimeout)时，[PAT](#pat) 动态使用（不超过预先分配的限制）和释放这些端口。

下表显示了针对后端池大小层的 SNAT 端口预分配：

| 池大小（VM 实例） | 每个 IP 配置的预先分配 SNAT 端口|
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1,000 | 32 |

>[!NOTE]
> 结合[多个前端](load-balancer-multivip-overview.md)使用标准负载均衡器时，上表中每个前端 IP 地址的可用 SNAT 端口数目将会倍增。 例如，如果某个后端池包含 50 个 VM 和 2 个负载均衡规则，并且每个 VM 具有不同的前端 IP 地址，则该后端池将根据规则使用 2048 (2 x 1024) 个 SNAT 端口。 参阅有关[多个前端](#multife)的详细信息。

请记住，可用的 SNAT 端口数不会直接转换为流数。 可以针对多个唯一目标重用单个 SNAT 端口。 仅当需要使流保持唯一时，才使用端口。 有关设计和缓解指导，请参阅[如何管理这项可耗尽的资源](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)；另请参阅介绍 [PAT](#pat) 的部分。

更改后端池大小可能会影响建立的某些流。 如果后端池大小递增并转换为下一层，则在转换为下一个更大的后端池层期间，一半的预先分配 SNAT 端口将被回收。 与回收的 SNAT 端口关联的流会超时，必须重新建立连接。 如果尝试新流，则只要预先分配的端口可用，则该流就能立即成功。

如果后端池减小并转换到更低层级，可用的 SNAT 端口数会增多。 在这种情况下，现有的分配 SNAT 端口及其相应的流不会受到影响。

SNAT 端口分配特定于 IP 传输协议（TCP 和 UDP 是分别维护的），并在以下条件下释放：

### <a name="tcp-snat-port-release"></a>TCP SNAT 端口释放

- 如果服务器/客户端均发送 FINACK，则 SNAT 端口在 240 秒后释放。
- 如果出现 RST，则 SNAT 端口在 15 秒后释放。
- 如果已达到空闲超时，则会释放端口。

### <a name="udp-snat-port-release"></a>UDP SNAT 端口释放

- 如果已达到空闲超时，则会释放端口。

## <a name="discovering-the-public-ip-that-a-vm-uses"></a><a name="discoveroutbound"></a>发现 VM 使用的公共 IP
有多种方法来确定出站连接的公共源 IP 地址。 OpenDNS 提供了一种服务可以向你显示 VM 的公共 IP 地址。 

使用 nslookup 命令，可以将名称 myip.opendns.com 的 DNS 查询发送到 OpenDNS 解析程序。 该服务返回用于发送此查询的源 IP 地址。 在 VM 中运行以下查询时，返回的是用于该 VM 的公共 IP。

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-outbound-connectivity"></a><a name="preventoutbound"></a>阻止出站连接
有时允许 VM 创建出站流是不可取的。 或者，可能需要管理哪些目标可以通过出站流访问或哪些目标可以启动入站流。 在此情况下，可以使用[网络安全组](../virtual-network/security-overview.md)管理 VM 可访问的目标。 还可以使用 NSG 来管理可启动入站流的公共目标。

将 NSG 应用于负载均衡的 VM 时，需要注意[服务标记](../virtual-network/security-overview.md#service-tags)和[默认安全规则](../virtual-network/security-overview.md#default-security-rules)。 必须确保 VM 可以接收来自 Azure 负载均衡器的运行状况探测请求。 

如果 NSG 阻止来自 AZURE_LOADBALANCER 默认标记的运行状况探测请求，那么 VM 的运行状况探测程序将失败，并且 VM 被标记为停机。 负载均衡器停止向此 VM 发送新流。

## <a name="connections-to-azure-storage-in-the-same-region"></a>连接到同一区域中的 Azure 存储

通过上述方案进行出站连接时不一定要连接到与 VM 位于同一区域的存储。 如果不想这样做，请按上述说明使用网络安全组 (NSG)。 若要连接到其他区域的存储，则需要使用出站连接。 请注意，当从同一区域中的虚拟机连接到存储时，存储诊断日志中的源 IP 地址将是内部提供程序地址，而不是虚拟机的公共 IP 地址。 如果要将对存储帐户的访问限制至同一区域中一个或多个虚拟网络子网中的 VM，请在配置存储帐户防火墙时使用[虚拟网络服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)，而不是公共 IP 地址。 配置了服务终结点后，将在存储诊断日志中看到虚拟网络专用 IP 地址，而不是内部提供程序地址。

## <a name="azure-load-balancer-outbound-rules"></a><a name="outboundrules"></a>Azure 负载均衡器出站规则

除了入站连接以外，Azure 负载均衡器还提供从虚拟网络的出站连接。  使用出站规则可以更方便地配置公共[标准负载均衡器](load-balancer-standard-overview.md)的出站网络地址转换。  你可以根据具体的需求，以完全声明性的方式控制出站连接，以缩放和优化此功能。

![负载均衡器出站规则](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

使用负载均衡器的出站规则可以： 
- 从头开始定义出站 NAT。
- 缩放和优化现有出站 NAT 的行为。 

使用出站规则可以控制：
- 哪些虚拟机应转换为哪些公共 IP 地址。 
- 应如何分配[出站 SNAT 端口](load-balancer-outbound-connections.md#snat)。
- 要为哪些协议提供出站转换。
- 用于出站连接空闲超时的持续时间（4-120 分钟）。
- 是否要在空闲超时时发送 TCP Reset

出站规则扩展了[出站连接](load-balancer-outbound-connections.md)一文中所述的[方案 2](load-balancer-outbound-connections.md#lb)，方案优先顺序保持不变。

### <a name="outbound-rule"></a>出站规则

与所有负载均衡器规则一样，出站规则遵循负载均衡和入站 NAT 规则的类似语法：

**前端** + **参数** + **后端池**

出站规则为后端池识别的、要转换为前端的所有虚拟机配置出站 NAT。   参数针对出站 NAT 算法提供更精细的控制。

API 版本“2018-07-01”允许按如下所示构建出站规则定义：

```json
      "outboundRules": [
        {
          "frontendIPConfigurations": [ list_of_frontend_ip_configuations ],
          "allocatedOutboundPorts": number_of_SNAT_ports,
          "idleTimeoutInMinutes": 4 through 66,
          "enableTcpReset": true | false,
          "protocol": "Tcp" | "Udp" | "All",
          "backendAddressPool": backend_pool_reference,
        }
      ]
```

>[!NOTE]
>有效出站 NAT 配置是所有出站规则与负载均衡规则的组合。 出站规则是对负载均衡规则的补充。 请查看[禁用负载均衡规则的出站 NAT](#disablesnat)，了解如何在将多个规则应用到 VM 时管理有效出站 NAT 转换。 在定义使用与负载均衡规则相同的公共 IP 地址的出站规则时，必须[禁用出站 SNAT](#disablesnat)。

#### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a>使用多个 IP 地址缩放出站 NAT

尽管出站规则只能配合单个公共 IP 地址使用，但出站规则减轻了缩放出站 NAT 的负担。 规划大规模方案时可以使用多个 IP 地址，并可以使用出站规则来缓解容易出现 [SNAT 耗尽](troubleshoot-outbound-connection.md#snatexhaust)的模式。  

前端提供的每个附加 IP 地址可提供 64,000 个临时端口，供负载均衡器用作 SNAT 端口。 尽管负载均衡规则或入站 NAT 规则具有单个前端，但出站规则可以扩展前端的概念，并允许为每个规则使用多个前端。  为每个规则使用多个前端时，可用 SNAT 端口的数量将与每个公共 IP 地址相乘，因此可以支持大型方案。

此外，可以直接对出站规则使用[公共 IP 前缀](https://aka.ms/lbpublicipprefix)。  使用公共 IP 前缀可以更轻松地缩放，并可简化将源自 Azure 部署的流加入允许列表的操作。 可以在负载均衡器资源中配置直接引用公共 IP 地址前缀的前端 IP 配置。  这样，负载均衡器将以独占方式控制公共 IP 前缀，而出站规则将自动使用公共 IP 前缀中包含的所有公共 IP 地址来建立出站连接。  公共 IP 前缀范围内的每个 IP 地址提供 64,000 个临时端口，供负载均衡器用作 SNAT 端口。   

使用此选项时，无法从公共 IP 前缀创建单个公共 IP 地址资源，因为出站规则必须拥有公共 IP 前缀的完全控制权。  如果需要更精细的控制，可以从公共 IP 前缀创建单个公共 IP 地址资源，并将多个公共 IP 地址单独分配到出站规则的前端。

#### <a name="tune-snat-port-allocation"></a><a name="snatports"></a>优化 SNAT 端口分配

可以使用出站规则[基于后端池大小优化自动 SNAT 端口分配](load-balancer-outbound-connections.md#preallocatedports)，并分配多于或少于自动 SNAT 端口分配所提供的端口数。

使用以下参数可为每个 VM 分配 10,000 个 SNAT 端口（NIC IP 配置）。
 

          "allocatedOutboundPorts": 10000

出站规则的所有前端中的每个公共 IP 地址最多提供 64,000 个可用作 SNAT 端口的临时端口。  负载均衡器以 8 的倍数分配 SNAT 端口。 如果提供的值不能被 8 整除，则会拒绝配置操作。  如果尝试分配的 SNAT 端口数超过了可用端口数（基于公共 IP 地址数确定），则会拒绝配置操作。  例如，如果为每个 VM 分配 10,000 个端口，并且后端池中的 7 个 VM 共享单个公共 IP 地址，则会拒绝该配置（7 x 10,000 个 SNAT 端口 > 64,000 个 SNAT 端口）。  将更多的公共 IP 地址添加到出站规则的前端即可实现该方案。

可以通过将端口数指定为 0，恢复为[基于后端池大小的自动 SNAT 端口分配](load-balancer-outbound-connections.md#preallocatedports)。 在这种情况下，根据该表，前 50 个 VM 实例将获得 1024 个端口，而 51-100 个 VM 实例将获得 512 个端口，依此类推。

#### <a name="control-outbound-flow-idle-timeout"></a><a name="idletimeout"></a>控制出站流空闲超时

出站规则提供一个配置参数用于控制出站流空闲超时，并使该超时符合应用程序的需求。  出站空闲超时默认为 4 分钟。  该参数接受从 4 到 120 的值用于指定与此特定规则匹配的流的空闲超时分钟数。

使用以下参数可将出站空闲超时设置为 1 小时：

          "idleTimeoutInMinutes": 60

#### <a name="enable-tcp-reset-on-idle-timeout"></a><a name="tcprst"></a> <a name="tcpreset"></a> 在空闲超时时启用 TCP 重置

负载均衡器的默认行为是在达到出站空闲超时时以静默方式丢弃流。  使用 enableTCPReset 参数可以启用更有预测性的应用程序行为，并控制在发生出站空闲超时时，是否要发送双向 TCP 重置 (TCP RST)。 

使用以下参数可在出站规则中启用 TCP 重置：

           "enableTcpReset": true

查看[在空闲超时时 TCP 重置](https://aka.ms/lbtcpreset)，了解详细信息，包括区域可用性。

#### <a name="support-both-tcp-and-udp-transport-protocols-with-a-single-rule"></a><a name="proto"></a>支持具有单个规则的 TCP 和 UDP 传输协议

可以对出站规则的传输协议使用“所有”，但也可以根据需要将出站规则应用到特定的传输协议。

使用以下参数可将协议设置为 TCP 和 UDP：

          "protocol": "All"

#### <a name="disable-outbound-nat-for-a-load-balancing-rule"></a><a name="disablesnat"></a>禁用负载均衡规则的出站 NAT

如前所述，负载均衡规则提供出站 NAT 的自动编程。 但是，某些方案受益于或者要求通过负载均衡规则禁用出站 NAT 的自动编程，以便能够控制或优化行为。  在某些出站规则方案中，必须停止自动出站 NAT 编程。

可通过两种方式使用此参数：
- （可选）禁止将入站 IP 地址用于出站 NAT。  出站规则是对负载均衡规则的补充，如果设置此参数，则出站规则将会受控。
  
- 优化同时用于入站和出站连接的 IP 地址的出站 NAT 参数。  必须禁用自动出站 NAT 编程才能让出站规则接管控制权。  例如，若要更改同时用于入站连接的某个地址的 SNAT 端口分配，则必须将此参数设置为 true。  如果尝试使用出站规则来重新定义同时用于入站连接的某个 IP 地址的参数，但尚未释放负载均衡规则的出站 NAT 编程，则配置出站规则的操作将会失败。

>[!IMPORTANT]
> 如果将此参数设置为 true，但没有任何出站规则（或[实例级公共 IP 方案](load-balancer-outbound-connections.md#ilpip)）定义出站连接，则虚拟机将无法访问公网。  VM或应用程序的某些操作可能依赖于公网连接。 请务必了解方案的依赖关系，并考虑此项更改造成的影响。

可以使用以下配置参数在负载均衡规则中禁用出站 SNAT：

```json
      "loadBalancingRules": [
        {
          "disableOutboundSnat": true
        }
      ]
```

disableOutboundSNAT 参数默认为 false，这意味着，负载均衡规则**确实**会提供自动出站 NAT 作为负载均衡规则配置的镜像。  

如果在负载均衡规则中将 disableOutboundSnat 设置为 true，则负载均衡规则将释放其他自动出站 NAT 编程的控制权。  出站 SNAT 随着负载均衡规则的启用而禁用。

#### <a name="reuse-existing-or-define-new-backend-pools"></a>重复使用现有后端池或定义新的后端池

出站规则没有引入有关定义要应用规则的 VM 组的新概念，  而重复使用后端池的概念（同样用于负载均衡规则）。 可以通过重复使用现有后端池定义或者为出站规则专门创建一个后端池，使用此概念来简化配置。

### <a name="scenarios"></a>方案

#### <a name="groom-outbound-connections-to-a-specific-set-of-public-ip-addresses"></a><a name="groom"></a>将出站连接整理成一组特定的公共 IP 地址

可以使用出站规则来整理出站连接，使之看上去像是源自一组特定的公共 IP 地址，以简化允许列表方案。  此源公共 IP 地址可与负载均衡规则使用的 IP 地址相同，也可以是与负载均衡规则使用的 IP 地址不同的一组公共 IP 地址。  

1. 创建[公共 IP 前缀](/load-balancer/load-balancer-outbound-connections#outboundrules)（或者从公共 IP 前缀创建公共 IP 地址）
2. 创建公共标准负载均衡器
3. 创建引用所要使用的公共 IP 前缀（或公共 IP 地址）的前端
4. 重复使用某个后端池或创建一个后端池，并将 VM 放入公共负载均衡器的后端池
5. 在公共负载均衡器中配置出站规则，以使用前端为这些 VM 的出站 NAT 编程
   
如果不希望将负载均衡规则用于出站连接，则需要在负载均衡规则中[禁用出站 SNAT](#disablesnat)。

#### <a name="modify-snat-port-allocation"></a><a name="modifysnat"></a>修改 SNAT 端口分配

可以使用出站规则[基于后端池大小优化自动 SNAT 端口分配](load-balancer-outbound-connections.md#preallocatedports)。

例如，如果你的两个虚拟机共享用于出站 NAT 的单个公共 IP 地址，则在遇到 SNAT 耗尽时，你可能希望增加分配的 SNAT 端口数，而不再使用默认的 1024 个端口。 每个公共 IP 地址最多可以提供 64,000 个临时端口。  如果使用单个公共 IP 地址前端配置出站规则，则总共可以向后端池中的 VM 分配 64,000 个 SNAT 端口。  对于两个 VM，可以使用出站规则最多分配 32,000 个 SNAT 端口 (2x32,000 = 64,000)。

查看[出站连接](load-balancer-outbound-connections.md)，以及有关如何分配和使用 [SNAT](load-balancer-outbound-connections.md#snat) 端口的详细信息。

#### <a name="enable-outbound-only"></a><a name="outboundonly"></a>仅启用出站连接

可以使用公共标准负载均衡器为一组 VM 提供出站 NAT。 在此方案中，可以单独使用出站规则，而无需其他任何规则。

##### <a name="outbound-nat-for-vms-only-no-inbound"></a>仅对 VM 使用出站 NAT（无入站连接）

定义一个公共标准负载均衡器，将 VM 放入后端池，配置一个出站规则用来为出站 NAT 编程，并整理出站连接，使其看上去源自特定的公共 IP 地址。 还可以使用公共 IP 前缀来简化出站连接源的允许列表操作。

1. 创建公共标准负载均衡器。
2. 创建一个后端池，并将 VM 放入公共负载均衡器的后端池。
3. 在公共负载均衡器中配置出站规则，以便为这些 VM 的出站 NAT 编程。

##### <a name="outbound-nat-for-internal-standard-load-balancer-scenarios"></a>内部标准负载均衡器方案的出站 NAT

使用内部标准负载均衡器时，只有显式声明出站连接之后，出站 NAT 才可用。 你可以通过以下步骤，使用出站规则为内部标准负载均衡器后面的 VM 创建出站连接，以定义出站连接：

1. 创建公共标准负载均衡器。
2. 除了内部负载均衡器，还要创建一个后端池，并将 VM 放入公共负载均衡器的后端池。
3. 在公共负载均衡器中配置出站规则，以便为这些 VM 的出站 NAT 编程。

##### <a name="enable-both-tcp--udp-protocols-for-outbound-nat-with-a-public-standard-load-balancer"></a>使用公共标准负载均衡器为出站 NAT 启用 TCP 和 UDP 协议

- 使用公共标准负载均衡器时，提供的自动出站 NAT 编程与负载均衡规则的传输协议相匹配。  

   1. 在负载均衡规则中禁用出站 SNAT。
   2. 在同一个负载均衡器上配置出站规则。
   3. 重复使用 VM 已用的后端池。
   4. 指定“协议”：“所有”作为出站规则的一部分。

- 只使用入站 NAT 规则时，不会提供出站 NAT。

   1. 将 VM 放入后端池。
   2. 使用公共 IP 地址或公共 IP 前缀定义一个或多个前端 IP 配置。
   3. 在同一个负载均衡器上配置出站规则。
   4. 指定“协议”：“所有”作为出站规则的一部分


## <a name="limitations"></a>限制
- 每个前端 IP 地址的最大可用临时端口数为 64,000。
- 可配置的出站空闲超时范围为 4 到 120 分钟（240 到 7200 秒）。
- 负载均衡器不支持将 ICMP 用于出站 NAT。
- 出站规则只能应用于 NIC 的主 IP 配置。  支持多个 NIC。
- 考虑到 VNet 出现之前的服务和其他平台服务的运行方式带来的副作用，只有在使用内部标准负载均衡器的情况下，才可以访问没有 VNet 和其他 Azure 平台服务的 Web 辅助角色。 请勿依赖此副作用，因为相应的服务本身或底层平台可能会在不通知的情况下进行更改。 在仅使用内部标准负载均衡器时，必须始终假定需要明确创建出站连接。 本文中所述的[默认 SNAT](#defaultsnat) 方案 3 不可用。

## <a name="next-steps"></a>后续步骤

- 详细了解[标准负载均衡器](load-balancer-standard-overview.md)。
- 详细了解标准公共负载均衡器的[出站规则](load-balancer-outbound-rules-overview.md)。
- 详细了解[负载均衡器](load-balancer-overview.md)。
- 详细了解[网络安全组](../virtual-network/security-overview.md)。
- 了解 Azure 的部分其他关键[网络功能](../networking/networking-overview.md)。
