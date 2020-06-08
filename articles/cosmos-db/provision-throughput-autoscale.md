---
title: 在自动缩放模式下创建 Azure Cosmos 容器和数据库。
description: 了解在自动缩放模式下预配 Azure Cosmos 数据库和容器的优势、用例和方法。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/11/2020
ms.date: 06/01/2020
ms.author: v-yeche
ms.openlocfilehash: 37f5a74f6a532386a67b87364af39eca9abafd9d
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200103"
---
<!--Verified successfully-->
# <a name="create-azure-cosmos-containers-and-databases-with-autoscale-throughput"></a>创建具有自动缩放吞吐量的 Azure Cosmos 容器和数据库

Azure Cosmos DB 允许在数据库和容器上设置标准（手动）或自动缩放预配吞吐量。 本文介绍自动缩放预配吞吐量的优势和用例。 

自动缩放预配吞吐量非常适合于具有可变或不可预测流量模式且需要高性能和大规模 SLA 的任务关键型工作负载。 

借助自动缩放，Azure Cosmos DB 可根据使用情况自动且即时地缩放数据库或容器的吞吐量 (RU/s)，而不会影响工作负载的可用性、延迟、吞吐量或性能。 

## <a name="benefits-of-autoscale"></a>自动缩放的好处

使用自动缩放预配吞吐量配置的 Azure Cosmos 数据库和容器具有以下优势：

* **Simple**：自动缩放消除了使用自定义脚本或手动缩放容量来管理 RU/s 的复杂性。 

* **可缩放：** 数据库和容器可根据需要自动缩放预配的吞吐量。 客户端连接和应用程序不会中断，且不影响 Azure Cosmos DB SLA。

* **经济高效：** 自动缩放可通过在不使用时进行纵向缩减来帮助优化 RU/s 和成本的使用。 只需按小时为工作负载需要的资源付费。

* **高度可用：** 使用自动缩放的数据库和容器利用相同的多区域分布式容错且高度可用的 Azure Cosmos DB 后端来确保数据持久性和高可用性。

## <a name="use-cases-of-autoscale"></a>自动缩放的用例

自动缩放的用例包括：

* **可变或不可预测的工作负载：** 当工作负载使用量出现可变或不可预测的峰值时，自动缩放将根据使用情况自动纵向扩展和缩减。 例如，流量模式因季节而异的零售网站；一天中多次出现峰值的 IOT 工作负载；每月或每年数次出现峰值使用量的商务应用程序等等。 使用自动缩放，不再需要手动预配峰值或平均容量。 

* **新应用程序：** 如果要开发新的应用程序，并且不确定所需的吞吐量 (RU/s)，可以通过自动缩放轻松入门。 可以从 400-4000 RU/s 的自动缩放入口点开始，在一段时间内监视使用情况，并确定正确的 RU/s。

* **不常用的应用程序：** 如果你每天、每周或每月只使用应用程序几次，每次几小时（例如低流量的应用程序/网站/博客站点），自动缩放可调整容量以处理峰值使用量并在高峰结束时进行纵向缩减。 

* **开发和测试工作负载：** 如果你或你的团队在工作时间使用 Azure Cosmos 数据库和容器，但在夜间或周末不使用，则自动缩放可通过在不使用时将其纵向缩减至最小来节省成本。 

* **计划的生产工作负荷/查询：** 如果你有一系列计划的请求、操作或查询要在空闲期间运行，则可以通过自动缩放轻松实现。 需要运行工作负载时，吞吐量将自动缩放到所需的大小，然后纵向缩减。 

针对这些问题构建自定义解决方案不仅需要花费大量时间，而且还会增加应用程序配置或代码的复杂性。 自动缩放可使上述方案开箱即用，而无需自定义或手动扩展容量。 

## <a name="how-autoscale-provisioned-throughput-works"></a>自动缩放预配吞吐量的工作原理

通过自动缩放配置容器和数据库时，请指定所需的最大吞吐量 `Tmax`。 Azure Cosmos DB 可缩放吞吐量 `T`（如 `0.1*Tmax <= T <= Tmax`）。 例如，如果将最大吞吐量设置为 20,000 RU/s，则吞吐量将在 2000-20,000 RU/s 之间缩放。 由于缩放是自动且即时的，因此可在任何时间点无延迟地使用吞吐量（且最多可使用预配的 `Tmax`）。 

每小时将向你收取系统在该小时内扩展到的最高吞吐量 `T` 的费用。

自动缩放最大吞吐量 `Tmax` 的入口点从 4000 RU/s 开始，在 400-4000 RU/s 之间缩放。 可以 1000 RU/s 为增量来设置 `Tmax`，并可随时更改该值。  

## <a name="enable-autoscale-on-existing-resources"></a>对现有资源启用自动缩放 ##
使用 [Azure 门户](how-to-provision-autoscale-throughput.md#enable-autoscale-on-existing-database-or-container)在现有数据库或容器上启用自动缩放。 可以随时在自动缩放和标准（手动）预配吞吐量之间切换。 有关详细信息，请参阅[此文档](autoscale-faq.md#how-does-the-migration-between-autoscale-and-standard-manual-provisioned-throughput-work)。

<a name="autoscale-limits"></a>
## <a name="throughput-and-storage-limits-for-autoscale"></a>自动缩放的吞吐量和存储限制

对于 `Tmax` 的任何值，数据库或容器均可以存储总计 `0.01 * Tmax GB`。 达到此存储量后，将根据新的存储值自动增加最大 RU/s，而不会影响应用程序。 

例如，如果开始时以 50,000 RU/s 为最大值（在 5000-50,000 RU/s 之间缩放），则最多可以存储 500 GB 的数据。 如果超过 500 GB，例如存储现在为 600 GB，则新的最大 RU/s 为 60,000 RU/s（在 6000-60,000 RU/s 之间缩放）。

使用可自动缩放的数据库级别吞吐量时，可以让前 25 个容器共享 4000 RU/s 的自动缩放最大值（在 400-4000 RU/s 之间缩放），前提是没有超过 40 GB 的存储空间。 有关详细信息，请参阅[此文档](autoscale-faq.md#can-i-change-the-max-rus-on-the-database-or-container)。

## <a name="comparison--containers-configured-with-manual-vs-autoscale-throughput"></a>配置了手动吞吐量与自动缩放吞吐量的容器比较
有关更多详细信息，请参阅此[文档](how-to-choose-offer.md)，了解如何在标准（手动）和自动缩放吞吐量之间进行选择。  

|| 具有标准（手动）吞吐量的容器  | 具有自动缩放吞吐量的容器 |
|---------|---------|---------|
| **预配吞吐量 (RU/s)** | 手动预配。 | 根据工作负荷使用模式自动即时缩放。 |
| **请求/操作的速率限制 (429)** | 如果消耗量超过预配的容量，则可能会发生。 | 如果使用的 RU/s 在设置的自动缩放吞吐量范围内，则不会发生。    |
| **容量规划** |  必须进行容量规划并预配所需的精确吞吐量。 |    系统会自动处理容量规划和容量管理。 |
| **价格** | 按小时费率为每小时使用的[标准（手动）RU/s](https://www.azure.cn/pricing/details/cosmos-db/) 付费。 | 每小时按系统在该小时内纵向扩展到的最大 RU/s 付费。 <br/><br/> 对于单写入区域帐户，需按小时费率为每小时使用的[自动缩放 RU/s](https://www.azure.cn/pricing/details/cosmos-db/) 付费。 <br/><br/>对于包含多个写入区域的帐户，自动缩放不收取额外的费用。 需按小时费率为每小时使用的相同[多主数据库 RU/s](https://www.azure.cn/pricing/details/cosmos-db/) 吞吐量付费。 |
| **最适合工作负荷类型** |  可预测的稳定工作负荷|   不可预测的可变工作负荷  |

## <a name="next-steps"></a>后续步骤

* 查看[自动缩放常见问题解答](autoscale-faq.md)。
* 了解如何[在手动和自动缩放吞吐量](how-to-choose-offer.md)之间进行选择。
* 了解如何[在 Azure Cosmos 数据库或容器上预配自动缩放吞吐量](how-to-provision-autoscale-throughput.md)。
* 详细了解 Azure Cosmos DB 中的[分区](partition-data.md)。

<!-- Update_Description: new article about provision throughput autoscale -->
<!--NEW.date: 06/01/2020-->