---
title: Azure Cosmos DB 中自动缩放预配吞吐量的常见问题解答
description: 有关 Azure Cosmos DB 数据库和容器的自动缩放预置吞吐量的常见问题解答
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 04/28/2020
ms.date: 06/15/2020
ms.author: v-yeche
ms.openlocfilehash: fc4568f1702efee7318f418580d68546acf57c23
ms.sourcegitcommit: 3de7d92ac955272fd140ec47b3a0a7b1e287ca14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723598"
---
<!--Verified successfully-->
# <a name="frequently-asked-questions-about-autoscale-provisioned-throughput-in-azure-cosmos-db"></a>Azure Cosmos DB 中自动缩放预配吞吐量的常见问题解答

使用自动缩放预配吞吐量，Azure Cosmos DB 将根据使用情况自动管理和缩放数据库或容器的 RU/s。 本文解答了有关自动缩放的常见问题。

## <a name="frequently-asked-questions"></a>常见问题

### <a name="is-autoscale-supported-for-all-apis"></a>是否所有 API 都支持自动缩放？
是的，所有 API 都支持自动缩放：Core (SQL)、Gremlin、Table、Cassandra 和 API for MongoDB。

### <a name="is-autoscale-supported-for-multi-master-accounts"></a>多主数据库帐户是否支持自动缩放？
是的，多主数据库帐户支持自动缩放。 添加到 Cosmos 帐户的每个区域都提供最大 RU/s。 

### <a name="what-is-the-pricing-for-autoscale-"></a>自动缩放采用何种定价方式？
有关详细信息，请参阅 Azure Cosmos DB [定价页](https://www.azure.cn/pricing/details/cosmos-db/)。 

### <a name="how-do-i-enable-autoscale-for-my-containers-or-databases"></a>如何为容器或数据库启用自动缩放？
可以在使用 Azure 门户创建的新容器和数据库上启用自动缩放。

### <a name="is-there-cli-or-sdk-support-to-create-containers-or-databases-with-autoscale-provisioned-throughput"></a>是否支持通过 CLI 或 SDK 创建配置了自动缩放吞吐量的容器或数据库？
目前，只能从 Azure 门户创建配置了自动缩放吞吐量的资源。 尚不支持 CLI 和 SDK。

### <a name="can-i-enable-autoscale-on-an-existing-container-or-a-database"></a>能否在现有容器或数据库上启用自动缩放？
目前，可以在创建新容器和数据库时对它们启用自动缩放。 尚不支持在现有容器和数据库上启用自动缩放。 可以使用 [Azure 数据工厂](../data-factory/connector-azure-cosmos-db.md)或[更改源](change-feed.md)将现有容器迁移到新容器。 

<a name="how-does-the-migration-between-autoscale-and-standard-manual-provisioned-throughput-work"></a>
### <a name="can-i-turn-off-autoscale-on-a-container-or-database"></a>能否在容器或数据库上禁用自动缩放？
是的，可以通过为预配吞吐量切换到“标准（手动）”选项来禁用自动缩放。 在当前版本中，从自动缩放吞吐量切换为标准预配吞吐量后，就不能再为同一资源启用自动缩放了。 

### <a name="is-autoscale-supported-for-shared-throughput-databases"></a>共享吞吐量数据库是否支持自动缩放？
是的，共享吞吐量数据库支持自动缩放。 若要启用此功能，请在创建数据库时选择自动缩放和“预配吞吐量”选项。 

### <a name="what-is-the-number-of-allowed-collections-per-shared-throughput-database-when-autoscale-is-enabled"></a>启用自动缩放后，每个共享吞吐量数据库允许的集合数是多少？
对于启用了自动缩放的共享吞吐量数据库，允许的集合数为：MIN(25, 数据库的最大 RU/s / 1000)。 例如，如果数据库的最大吞吐量为 20,000 RU/s，则数据库可以有 MIN(25, 20,000 RU/s / 1000) = 20 个集合。 

### <a name="what-is-the-storage-limit-associated-with-each-max-rus-option"></a>与每个最大 RU/s 选项关联的存储限制是多少？  
每个最大 RU/s 的存储限制（以 GB 为单位）为：数据库或容器的最大 RU/s / 100。 例如，如果最大 RU/s 为 20,000 RU/s，则资源可支持 200 GB 的存储。 请参阅[自动缩放限制](provision-throughput-autoscale.md#autoscale-limits)一文，了解可用的最大 RU/s 和存储选项。 

### <a name="what-happens-if-i-exceed-the-storage-limit-associated-with-my-max-throughput"></a>如果超出与最大吞吐量关联的存储限制，会发生什么情况？
如果超出了与数据库或容器的最大吞吐量关联的存储限制，Azure Cosmos DB 会自动将最大吞吐量提高到可以支持该级别存储的下一个最高层级。 例如，假设为数据库或容器预配了 4000 RU/s 的最大吞吐量选项，该选项的存储限制为 50 GB。 如果资源存储增加到 100 GB，则数据库或容器的最大 RU/s 将自动增加到 20,000 RU/s（最多可支持 200 GB）。 

### <a name="how-quickly-will-autoscale-up-and-down-based-on-spikes-in-traffic"></a>自动缩放根据流量高峰进行增减的速度有多快？
利用自动缩放，可以根据传入流量在最小和最大 RU/s 范围内立即增加或减少 RU/s。 计费时间间隔为 1 小时，即，按某一小时内的最高 RU/s 收费。

### <a name="can-i-specify-a-custom-max-throughput-rus-value-for-autoscale"></a>能否为自动缩放指定自定义最大吞吐量 (RU/s) 值？
目前，有[四个最大吞吐量 (RU/s) 选项](provision-throughput-autoscale.md#autoscale-limits)可供选择。

<a name="can-i-change-the-max-rus-on-the-database-or-container"></a>
### <a name="can-i-increase-the-max-rus-move-to-a-higher-tier-on-the-database-or-container"></a>能否增加数据库或容器的最大 RU/s（移至较高层级）？ 
是的。 可以从容器的“缩放和设置”选项或数据库的“缩放”选项中，为自动缩放选择更高的最大 RU/s 。 这是一个异步增加操作，随着服务预配更多资源来支持更高的规模，该操作可能需要一段时间才能完成（通常为 4-6 小时，具体取决于所选的 RU/s）。 

### <a name="can-i-reduce-the-max-rus-move-to-a-lower-tier-on-the-database-or-container"></a>能否降低数据库或容器的最大 RU/s（移至较低层级）？
是的。 只要数据库或容器的当前存储低于要缩减到的最大 RU/s 层级的相关[存储限制](#what-is-the-storage-limit-associated-with-each-max-rus-option)，就可以将最大 RU/s 降低到该层级。 例如，如果选择 20,000 RU/s 作为最大 RU/s，但存储小于 50 GB（与 4000 RU/s 关联的存储限制），则可以将最大 RU/s 缩减为 4000 RU/s。

### <a name="what-is-the-mapping-between-the-max-rus-and-physical-partitions"></a>最大 RU/s 与物理分区之间的映射是什么？
首次选择最大 RU/s 时，Azure Cosmos DB 将进行以下预配：最大 RU/s / 10,000 RU/s = 物理分区数。 每个[物理分区](partition-data.md#physical-partitions)最多可支持 10,000 RU/s 和 50 GB 的存储。 随着存储大小的增长，Azure Cosmos DB 将自动拆分分区，以添加更多物理分区来应对存储的增长；如果存储[超过相关限制](#what-is-the-storage-limit-associated-with-each-max-rus-option)，Azure Cosmos DB 将增加最大 RU/s 层级。 

数据库或容器的最大 RU/s 在所有物理分区之间平均分配。 因此，任何单个物理分区可以扩展到的总吞吐量为：数据库或容器的最大 RU/s / 物理分区数。 

### <a name="what-happens-if-incoming-requests-exceed-the-max-rus-of-the-database-or-container"></a>如果传入请求超过数据库或容器的最大 RU/s，会发生什么情况？
如果 RU/s 总消耗量超过容器或数据库的最大 RU/s，则超过最大 RU/s 的请求将受到限制并返回 429 状态代码。 导致标准化利用率超过 100% 的请求也将受到限制。 标准化利用率定义为所有物理分区中 RU/s 利用率的最大值。 例如，假设最大吞吐量为 20,000 RU/s，并且有两个物理分区 P_1 和 P_2，每个分区都可以扩展到 10,000 RU/s。 在给定的秒数内，如果 P_1 已使用 6000 RU，而 P_2 已使用 8000 RU，则标准化利用率为 MAX(6000 RU / 10,000 RU, 8000 RU / 10,000 RU) = 0.8。

> [!NOTE]
> Azure Cosmos DB 客户端 SDK 和数据导入工具（Azure 数据工厂、批量执行程序库）会在出现 429 错误时自动重试，因此，偶尔出现 429 错误是正常的。 持续大量的 429 错误可能表明你需要提高最大 RU/s 或查看[热分区](#autoscale-rate-limiting)的分区策略。

<a name="autoscale-rate-limiting"></a>
### <a name="is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autoscale-is-enabled"></a>启用自动缩放后，是否仍有可能看到 429 错误（限制/速率限制）？ 
是的。 在两种情况下有可能看到 429 错误。 第一种，当 RU/s 总消耗量超过容器或数据库的最大 RU/s 时，服务将相应地限制请求。 

第二种，如果存在热分区，即与其他分区键值相比，请求数量高到不成比例的逻辑分区键值，则基础物理分区可能会超出其 RU/s 预算。 为避免出现热分区，最佳做法是[选择一个合适的分区键](partitioning-overview.md#choose-partitionkey)，以实现存储和吞吐量的均匀分配。 

例如，如果选择 20,000 RU/s 的最大吞吐量选项，并具有 200 GB 的存储和四个物理分区，则每个物理分区最多可以自动扩展至 5000 RU/s。 如果特定逻辑分区键上有一个热分区，则当它所在的基础物理分区超过 5000 RU/s（即超过 100% 的标准化利用率）时，将显示 429 错误。

## <a name="next-steps"></a>后续步骤

* 了解如何[在 Azure Cosmos 容器或数据库上启用自动缩放](provision-throughput-autoscale.md)。
    
    <!--Not Available on #create-db-container-autoscale-->
    
* 了解[使用自动缩放预配吞吐量的优势](provision-throughput-autoscale.md#autoscale-benefits)。
* 详细了解[逻辑分区和物理分区](partition-data.md)。

<!-- Update_Description: update meta properties, wording update, update link -->