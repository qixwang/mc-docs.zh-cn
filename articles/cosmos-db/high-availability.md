---
title: Azure Cosmos DB 中的高可用性
description: 本文介绍 Azure Cosmos DB 如何提供高可用性
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 12/06/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.reviewer: sngun
ms.openlocfilehash: c53eb54a64c8cdd8d538224a5c2019e5f518deb0
ms.sourcegitcommit: 23dc63b6fea451f6a2bd4e8d0fbd7ed082ba0740
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980526"
---
# <a name="high-availability-with-azure-cosmos-db"></a>使用 Azure Cosmos DB 实现高可用性

Azure Cosmos DB 以透明方式在与 Cosmos 帐户关联的所有 Azure 区域之间复制数据。 Cosmos DB 对数据采用多层冗余，如下图所示：

![物理分区](./media/high-availability/cosmosdb-data-redundancy.png)

- Cosmos 容器中的数据[已水平分区](partitioning-overview.md)。

- 在每个区域中，每个分区受副本集的保护，该副本集中的大多数副本将复制并以持久方式提交所有写入内容。 副本分布在最多 10 到 20 个容错域中。

- 将复制所有区域中的每个分区。 每个区域包含某个 Cosmos 容器的所有数据分区，可接受写入并为读取提供服务。  

如果 Cosmos 帐户分布在 *N* 个 Azure 区域之间，则所有数据至少有 *N* x 4 个副本。 除了在与 Cosmos 帐户关联的区域之间提供低延迟数据访问和缩放写入/读取吞吐量以外，部署更多的区域（*N* 值较高）还可进一步提高可用性。  

## <a name="slas-for-availability"></a>可用性 SLA

作为多区域分布式数据库，Cosmos DB 提供综合性的 SLA，涵盖了吞吐量、99% 时间内的延迟、一致性和高可用性。 下表显示了 Cosmos DB 针对单区域和多区域帐户提供的高可用性保证。 为实现高可用性，请始终将 Cosmos 帐户配置为使用多个写入区域。

|操作类型  | 单区域 |多区域（单区域写入）|多区域（多区域写入） |
|---------|---------|---------|-------|
|写入    | 99.99    |99.99   |99.999|
|读取     | 99.99    |99.999  |99.999|

> [!NOTE]
> 在实践中，有限过期、会话、一致前缀和最终一致性模型的实际写入可用性明显高于发布的 SLA。 所有一致性级别的实际读取可用性明显高于发布的 SLA。

## <a name="high-availability-with-cosmos-db-in-the-event-of-regional-outages"></a>使用 Cosmos DB 在遇到区域性服务中断时提供高可用性

区域性服务中断并不少见，而 Azure Cosmos DB 可确保你的数据库始终保持高可用性。 下面根据 Cosmos 帐户配置详细汇总了 Cosmos DB 在服务中断期间的行为：

- 使用 Cosmos DB 时，在客户端确认写入操作之前，数据将由接受写入操作的区域中的副本仲裁持久提交。

- 配置有多个写入区域的多区域帐户对于写入和读取都将具有高可用性。 区域性故障转移可在瞬间完成，不需要在应用程序中进行任何更改。

- 发生区域性服务中断时，单区域帐户可能会失去可用性。 建议始终对 Cosmos 帐户**至少设置两个区域**（最好是至少设置两个写入区域），以确保始终保持高可用性。

- **配置为使用单个写入区域的多区域帐户（写入区域服务中断）：**
    - 在写入区域中断期间，如果在 Azure Cosmos 帐户上配置了**启用自动故障转移**，则 Cosmos 帐户会将次要区域自动提升为新的主要写入区域。 当启用后，将按您指定的区域优先级顺序故障转移到其他区域。
    - 客户还可以选择使用**手动故障转移**并使用他们自己构建的代理亲自监视其 Cosmos 写入终结点 URL。 对于具有复杂和精密的运行状况监视需求的客户，这可以降低当写入区域发生故障时的 RTO。
    - 当上一个受影响的区域重新联机时，可以通过[冲突源](how-to-manage-conflicts.md#read-from-conflict-feed)来使用该区域发生故障时未复制的任何写入数据。 应用程序可以读取冲突源，根据应用程序特定的逻辑解决冲突，并相应地将更新后的数据写回 Azure Cosmos 容器。
    - 以前受影响的写入区域恢复后，它将自动用作读取区域。 可以切换回到用作写入区域的已恢复区域。 可以使用 [Azure CLI 或 Azure 门户](how-to-manage-database-account.md#manual-failover)来切换区域。 在切换写入区域之前、期间或之后，**不会丢失数据或可用性**，应用程序将继续保持高可用性。

- **配置为使用单个写入区域的多区域帐户（读取区域服务中断）：**
    - 在发生读取区域服务中断期间，这些帐户将保持很高的读写可用性。
    - 受影响的区域将自动断开连接，并标记为脱机。 [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) 会将读取调用重定向到首选区域列表中的下一个可用区域。
    - 如果首选区域列表中没有区域可用，则会自动让调用返回到当前的写入区域。
    - 处理读取区域服务中断不需要对应用程序代码进行更改。 最终，当受影响区域重新联机时，以前受影响的读取区域将自动与当前写入区域同步，并再次可用于为读取请求提供服务。
    - 后续的读取会重定向到恢复的区域，不需更改应用程序代码。 在故障转移和重新加入以前发生故障的区域期间，Cosmos DB 会持续遵循读取一致性保证。

- 即使在罕见的不幸事件中，发生了 Azure 区域永久无法恢复的情况，如果为多区域 Cosmos 帐户配置了默认的非常一致性，也不会丢失数据。  如果出现永久不可恢复的写入区域，对于配置了有限过期一致性的多区域 Cosmos 帐户，潜在的数据丢失时段限制为过期时段（*K* 或 *T*），其中 K = 100,000 次更新，T = 5 分钟。 对于会话、一致前缀和最终一致性级别，潜在的数据丢失时段限制为最多 15 分钟。 有关 Azure Cosmos DB 的 RTO 和 RPO 目标的详细信息，请参阅[一致性级别和数据持续性](consistency-levels-tradeoffs.md#rto)

<!--Not Avaialble on ## Availability Zone support-->



## <a name="building-highly-available-applications"></a>生成高可用性应用程序

- 为确保较高的写入和读取可用性，请将 Cosmos 帐户配置为跨越多个写入区域中的至少两个区域。 对于读取和写入，此配置都可提供由 SLA 作为保障的最高可用性、最低延迟和最佳可伸缩性。 详细了解如何[将 Cosmos 帐户配置为使用多个写入区域](tutorial-global-distribution-sql-api.md)。

- 对于配置为使用单个写入区域的多区域 Cosmos 帐户，请[使用 Azure CLI 或 Azure 门户中启用自动故障转移](how-to-manage-database-account.md#automatic-failover)。 启用自动故障转移后，每当发生区域性灾难时，Cosmos DB 都会自动故障转移你的帐户。  

- 即使 Cosmos 帐户具有高可用性，应用程序也不一定能够正常保持高可用性。 若要测试应用程序的端到端高可用性，请在应用程序测试或灾难恢复 (DR) 演练过程中，暂时禁用帐户的自动故障转移，[使用 Azure CLI 或 Azure 门户调用手动故障转移](how-to-manage-database-account.md#manual-failover)，然后监视你的应用程序的故障转移。 完成后，可以故障回复到主区域，然后还原该帐户的自动故障转移。

- 在多区域分布式数据库环境中，当发生区域范围的服务中断时，一致性级别与数据持续性之间存在直接关系。 制定业务连续性计划时，需了解应用程序在中断事件发生后完全恢复之前的最大可接受时间。 应用程序完全恢复所需的时间称为恢复时间目标 (RTO)。 此外，还需要了解从中断事件恢复时，应用程序可忍受最近数据更新丢失的最长期限。 可以承受更新丢失的时限称为恢复点目标 (RPO)。 若要查看 Azure Cosmos DB 的 RPO 和 RTO，请参阅[一致性级别和数据持续性](consistency-levels-tradeoffs.md#rto)

## <a name="next-steps"></a>后续步骤

接下来可以阅读以下文章：

- [各种一致性级别的可用性和性能权衡](consistency-levels-tradeoffs.md)
- [全局缩放预配的吞吐量](scaling-throughput.md)
- [多区域分布 - 揭秘](global-dist-under-the-hood.md)
- [Azure Cosmos DB 中的一致性级别](consistency-levels.md)
- [如何将 Cosmos 帐户配置为使用多个写入区域](how-to-multi-master.md)

<!-- Update_Description: update meta properties, wording update, update link -->
