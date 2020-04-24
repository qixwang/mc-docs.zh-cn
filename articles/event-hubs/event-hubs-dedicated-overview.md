---
title: 专用事件中心概述 - Azure 事件中心 | Azure
description: 本文概述专用 Azure 事件中心，它提供事件中心的单租户部署。
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
origin.date: 12/06/2018
ms.date: 12/02/2019
ms.author: v-tawe
ms.openlocfilehash: 443e726aec502331e8f4cecd8930595651ff135d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74658007"
---
<!--event hubs dedicate is supported in mooncake, but event hubs cluster not availible, so don't update this content-->
<!--This article do not need to update-->
# <a name="overview-of-event-hubs-dedicated"></a>专用事件中心概述

*专用事件中心* 容量提供单租户部署。 完整规模的 Azure 事件中心可每秒传入超过两百万个事件或每秒高达 2 GB 的遥测，并具有完全持久的存储和次秒级的延迟。 通过在相同系统上实时和批处理，它还实现了集成的解决方案。 借助包含在产品中的[事件中心捕获](event-hubs-capture-overview.md)功能，单个流可以同时支持实时和基于批处理的管道，从而降低解决方案的复杂性。

下表比较了事件中心的各可用服务层级。 不同于标准事件中心中大部分功能的使用定价，事件中心专用套餐每月的计费价格是固定的。 专用层提供标准计划的所有功能，但具有企业规模容量，以满足客户的工作负荷需求。 

| Feature | Standard | 专用 |
| --- |:---:|:---:|
| 带宽 | 20 TU（最多 40 TU） | 20 CU |
| 命名空间 |  1 | 每个 CU 50 |
| 事件中心 |  每个命名空间 10 | 每个命名空间 1000 |
| 入口事件 | 按每百万个事件支付 | 附送 |
| 消息大小 | 1000000 字节 | 1000000 字节 |
| “度量值组” | 每个命名空间 40 | 每个 CU 2000 |
| 使用者组 | 每个事件中心 20 | 每个 CU 无限制，每个事件中心 1000 |
| 中转连接 | 包括 1,000 个，最大 5,000 个 | 包括 100000 个，最大 100000 个 |
| 消息保留 | 7 天，每个 TU 包含 84 GB | 90 天，每个 CU 包含 10 TB |
| 捕获 | 按每小时支付 | 附送 |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>专用事件中心容量的优点

使用专用事件中心具有以下优点：

* 单个租户托管，免除来自其他租户的干扰。
* 每次可重复性能。
* 有保障的容量，满足迸发需求。
* 包括事件中心的[捕获](event-hubs-capture-overview.md)功能，提供与微批处理和长期保留的集成。
* 零维护：该服务负责管理负载均衡、操作系统更新、安全修补程序及分区。
* 固定小时定价。
* 消息保留期长达 7 天，无需支付额外费用。

专用事件中心还去除了标准产品的部分吞吐量限制。 基本层的吞吐量单位可达每秒 1000 个事件，或者每个 TU 每秒 1 MB 的流入量和两倍的流出量。 专用规模产品对入口和出口事件计数不设限制。 这些限制仅由购买的事件中心处理容量管理。

此保留的专用环境提供该层独有的其他功能，例如：

* 控制群集中的命名空间数量。
* 指定每个命名空间的吞吐量限制。
* 配置每个命名空间下事件中心数量。
* 确定分区的数量限制。

该服务针对最大的遥测用户，也可提供给具有企业协议的客户。

## <a name="how-to-onboard"></a>如何加入

通过添加或删除 CU，可以在一个月内随时扩展或缩小容量，满足自身需求。 专用计划独一无二，用户可从事件中心产品团队处获得适合自己的灵活部署，提供一种亲身实践操作体验。 若要加入到此 SKU，请[联系计费支持部门](https://support.azure.cn/support/support-azure/)或 Azure 代表。

## <a name="next-steps"></a>后续步骤

请与 Azure 销售代表或 Azure 支持部门联系，获取有关事件中心专用容量的其他详细信息。 还可访问以下链接，了解有关事件中心定价层的详细信息：

- [专用事件中心定价](https://www.azure.cn/pricing/details/event-hubs/)。 还可以与 Azure 销售代表或 Azure 支持部门联系，获取有关事件中心专用容量的其他详细信息。
- [事件中心常见问题解答](event-hubs-faq.md)中包含了定价信息并解答了一些有关事件中心的常见问题。
