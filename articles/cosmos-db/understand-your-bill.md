---
title: 了解 Azure Cosmos DB 帐单
description: 本文通过一些示例介绍如何了解 Azure Cosmos DB 帐单。
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 08/01/2019
ms.date: 02/10/2020
ms.reviewer: sngun
ms.openlocfilehash: 1bae6d1799ccb35774c8c31c321b61b237b5aadd
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77067887"
---
# <a name="understand-your-azure-cosmos-db-bill"></a>了解 Azure Cosmos DB 帐单

Azure Cosmos DB 是完全托管的云原生数据库服务，仅针对预配的吞吐量和消耗的存储收费，从而简化计费。 与本地或 IaaS 托管的替代方案相比，无需额外的许可费、硬件、使用成本或设施成本。 若想使用 Azure Cosmos DB 的多区域功能，与现有本地或 IaaS 解决方案相比，数据库服务可显着降低成本。

使用 Azure Cosmos DB 时，需根据预配的吞吐量和使用的存储按小时付费。 对于预配的吞吐量，计费单位为每小时 100 RU/秒，每小时收取 0.082 元（假定为标准公开定价），请参阅[定价页面](https://www.azure.cn/pricing/details/cosmos-db/)。 对于消耗的存储，每月每 1 GB 存储收取 2.576 元，请参阅[定价页面](https://www.azure.cn/pricing/details/cosmos-db/)。 

本文通过一些示例来帮助你了解每月帐单上的详细信息。 若存在以下情况，则示例中显示的数字可能会有所不同：Azure Cosmos 容器预配的吞吐量不同；容器跨多个区域；或在一个月内在不同时期运行。

> [!NOTE]
> 计费针对挂钟时间的任何部分，而不是 60 分钟的持续时间。

## <a name="billing-examples"></a>计费示例

### <a name="billing-example---throughput-on-a-container-full-month"></a>计费示例 – 容器上的吞吐量（整月）

* 假设在容器上配置了 1,000 RU/秒的吞吐量，且在该月总共存在 24 小时* 31 天 = 744 小时。  

* 容器每小时存在的 1,000 RU/秒是 10 个每小时 100 RU/秒（即 1,000/100 = 10）。 

* 以每小时 10 个单位乘以成本 0.082 元（每小时每 100 RU/秒）= 每小时 0.82 元。 

* 以每小时 0.82 元乘以当月的小时数，即 0.82 * 24 小时 * 31 天 = 当月 610.1 元。  

* 月度总帐单将显示 7,440 个单位（100 个 RU），费用为 610.1 元。

### <a name="billing-example---throughput-on-a-container-partial-month"></a>计费示例 – 容器上的吞吐量（不足一个月）

* 假设创建一个预配吞吐量为 2,500 RU/秒的容器。容器在一个月内存在 24 小时（例如，在创建容器后 24 小时将其删除）。  

* 则帐单上会显示 600 个单位（2,500 RU/秒/100 RU/秒/单位* 24小时）。 费用为 49.2 元（600 个单位 * 0.082 元/单位）。

* 当月总帐单将为 49.2 元。

### <a name="billing-rate-if-storage-size-changes"></a>存储大小变化时的计费率

存储容量按一个月内每小时的最大数据存储量（以 GB 为单位）计费。 例如，如果前半个月使用了 100 GB 的存储空间，而后半个月使用了 50 GB 的存储空间，则该月将按 75 GB 的等效存储空间进行计费。

### <a name="billing-rate-when-container-or-a-set-of-containers-are-active-for-less-than-an-hour"></a>容器或一组容器活动时间在一小时内的计费率

将对容器或数据库存在的每小时按统一费率进行收费，而无论使用量是多少，也无论容器或数据库使用时间是否不足一个小时。 例如，如果创建一个容器或数据库，然后在 5 分钟后删除它，那么帐单显示 1 个小时的费用。

### <a name="billing-rate-when-throughput-on-a-container-or-database-scales-updown"></a>容器或数据库上的吞吐量扩展/缩减时的计费率

如果在上午 9:30 将预配的吞吐量从 400 RU/秒增加到 1,000 RU/秒，然后在上午 10:45 将预配的吞吐量重新减少到 400 RU/秒，则将收取两小时 1,000 RU/秒的费用。 

如果在上午 9:30 将某个容器或一组容器的预配吞吐量从 100K RU/秒增加到 200K RU/秒，然后在上午 10:45 将预配的吞吐量重新减少到 100K RU/秒，则将收取两小时 200K RU/秒的费用。

### <a name="billing-example-multiple-containers-each-with-dedicated-provisioned-throughput-mode"></a>计费示例：多个容器，每个容器都采用专用的预配吞吐量模式

* 如果在中国东部 2 区域创建包含两个容器的 Azure Cosmos 帐户，并且这两个容器分别预配了 500 RU/秒和 700 RU/秒的吞吐量，则预配的总吞吐量将为 1,200 RU/秒。  

* 需支付 1,200/100 * 0.082 元 = 0.984 元/小时。 

* 如果需要更改吞吐量，且每个容器的容量增加了 500 RU/秒，同时还以 20,000 RU/秒创建了新的无限容器，则预配的总体容量为 22,200 RU/秒（1,000 RU/秒 + 1,200 RU/秒 + 20,000 RU/秒）。  

* 此时需支付： 0.082 元 x 222 = 18.21 元/小时。 

* 假设一个月为 744 小时（24 小时 * 31 天），如果 500 小时的预配吞吐量为 1,200 RU/秒，其余 244 小时的预配吞吐量为 22,200 RU/秒，则月度帐单将显示：500 x 0.984 元/小时 + 244 x 18.21元/小时 = 13402.56 元/月。

    <!--Not Available on ![Dedicated throughput bill example](./media/understand-your-bill/bill-example1.png)-->

### <a name="billing-example-containers-with-shared-throughput-mode"></a>计费示例：具有共享吞吐量模式的容器

* 如果在中国东部 2 区域创建包含两个 Azure Cosmos 数据库（其中含有一组在数据库级别共享吞吐量的容器）的 Azure Cosmos 帐户，并且这两个容器分别预配了 50K RU/秒和 70K RU/秒的吞吐量，则预配的总吞吐量将为 120K RU/秒。  

* 需支付 1200 x 0.082 元 = 98.4 元/小时。 

* 如果需要更改吞吐量，且每个数据库的预配吞吐量增加了 10K RU/秒，同时还向第一个数据库添加了新容器，共享吞吐量数据库的专用吞吐量模式为 15K RU /秒，则预配的总体容量为 155K RU/秒（60K RU/秒 + 80K RU/秒 + 15K RU/秒）。  

* 此时需支付：1,550 * 0.082 元 = 127.10 元/小时。  

* 假设一个月为 744 小时，如果 300 小时的预配吞吐量为 120K RU/秒，其余 444 小时的预配吞吐量为 155K RU/秒，则月度帐单将显示：300 x 98.4 元/小时 + 444 x 127.10 元/小时 = 29,520 元 + 56,433 元 = 85,953 元/月。 

    <!--Not Available on ![Shared throughput bill example](./media/understand-your-bill/bill-example2.png)-->

## <a name="billing-examples-with-geo-replication-and-multi-master"></a>采用异地复制和多主数据库的计费示例  

可随时向 Azure Cosmos 数据库帐户添加/删除中国 Azure 区域。 对于为不同 Azure Cosmos 数据库和容器配置的吞吐量，可在与 Azure Cosmos 数据库帐户关联的每个 Azure 区域中保留这些吞吐量。 如果在 Azure Cosmos 数据库帐户（按小时配置）中的所有数据库和容器中配置的总预配吞吐量（RU /秒）为 T，而与数据库帐户关联的 Azure 区域数为 N，则对于 Azure Cosmos 数据库帐户而言，在给定时间内（小时）总预配吞吐量分别为：(a) 配置了单个写入区域时，总吞吐量 = T x N RU/秒；(b) 配置了所有能够处理写入的区域时，总吞吐量 = T x (N + 1) RU/每秒。 预配吞吐量（单个写入区域）每 100 RU/秒的费用为 0.082 元/小时，对于多个写入区域（配置多主数据库），每 100 RU/秒的费用为 0.102 元/小时（参阅[定价页面](https://www.azure.cn/pricing/details/cosmos-db/)）。 无论是单个写入区域还是多个写入区域，Azure Cosmos DB 都支持从任何区域读取数据。

### <a name="billing-example-multi-region-azure-cosmos-account-single-region-writes"></a>计费示例：多区域 Azure Cosmos 帐户，单个区域写入

假定存在位于中国北部的 Azure Cosmos 容器。 创建容器时，指定的吞吐量为 10K RU/秒，本月可存储 1 TB 的数据。 假定向 Azure Cosmos 帐户添加 3 个区域（中国东部、中国北部 2 和中国东部 2），每个区域都具有相同的存储和吞吐量。 则月度帐单为（假定一个月 31 天）。 帐单情况如下： 

|**项目** |**使用情况（月）** |**费率** |**每月成本** |
|---------|---------|---------|-------|
|中国北部容器的吞吐量帐单      | 10K RU/秒 * 24 * 31    |每 100 RU/秒每小时 0.082 元   |6,101 元|
|3 个其他区域 - 中国东部、中国北部 2 和中国东部 2 的吞吐量帐单       | 3 * 10K RU/秒 * 24 * 31    |每 100 RU/秒每小时 0.082 元  |18,303 元|
|中国北部容器的存储帐单      | 250 GB    |2\.576 元/GB  |644 元|
|3 个其他区域 - 中国东部、中国北部 2 和中国东部 2 的存储帐单      | 3 * 250 GB    |2\.576 元/GB  |1,932 元|
|**总计**     |     |  |**26,980 元**|

此外，假定每月从中国北部的容器中传出 100 GB 数据，将数据复制到中国东部、中国北部 2 和中国东部 2。  则需要按数据传输速率为导出部分付费。

### <a name="billing-example-multi-region-azure-cosmos-account-multi-region-writes"></a>计费示例：多区域 Azure Cosmos 帐户，多个区域写入

假定创建位于中国北部的 Azure Cosmos 容器。 创建容器时，指定的吞吐量为 10K RU/秒，本月可存储 1 TB 的数据。 假定添加 3 个区域（中国东部、中国北部 2 和中国东部 2），每个区域的存储和吞吐量都相同，并且你希望能够写入到与 Azure Cosmos 帐户关联的所有区域中的容器。 则月度帐单（假定一个月 31 天）情况如下：

<!--Should be China East, China North 2, China East 2-->

|**项目** |**使用情况（月）**|**费率** |**每月成本** |
|---------|---------|---------|-------|
|中国北部容器的吞吐量帐单（所有区域都可写入）       | 10K RU/秒 * 24 * 31    |每 100 RU/秒每小时 0.102 元    |7589 元 |
|3 个其他区域 - 中国东部、中国北部 2 和中国东部 2（所有区域均可写入）的吞吐量帐单        | (3 + 1) * 10K RU/秒 * 24 * 31    |每 100 RU/秒每小时 0.102 元   |30,355.2 元 |
|中国北部容器的存储帐单      | 250 GB    |2\.576 元/GB  |644 元|
|3 个其他区域 - 中国东部、中国北部 2 和中国东部 2 的存储帐单      | 3 * 250 GB    |2\.576 元/GB  |1,932 元|
|**总计**     |     |  |**40,520.2 元**|

此外，假定每月从中国北部的容器中传出 100 GB 数据，将数据复制到中国东部、中国北部 2 和中国东部 2。  则需要按数据传输速率为导出部分付费。

<!--Not Available on ### Billing example: Azure Cosmos account with multi-master, database-level throughput including dedicated throughput mode for some containers-->

## <a name="proactively-estimating-your-monthly-bill"></a>主动估算每月帐单  

我们来看看另一个示例，介绍希望在月末之前主动估算帐单的情况。 可按如下方式估算帐单：

|**存储成本** | |
|----|----|
|平均记录大小 (KB) |1 |
|记录数  |100,000,000  |
|总存储 (GB)  |100 |
|每 GB 的月度成本  |2\.576 元  |
|预期的月度存储成本   |257.6 元  |

<br />

|**吞吐量成本** | | | |
|----|----|----|----|
|操作类型| 请求数/秒| 平均值RU/请求| 所需 RU|
|写入| 100 | 5 | 500|
|读取| 400| 1| 400|

总 RU/秒：500 + 400 = 900；每小时成本：900/100 * 0.082 元 = 0.738 元；预期的月度吞吐量成本（假定每月 31 天）：0.738 元 * 24 * 31 = 549.1 元

**每月总成本**

月度总成本 = 月度存储成本 + 月度吞吐量成本；月度总成本 = 257.6 元 + 549.1 元 = 806.7 元

区域不同定价可能有所不同。  有关最新定价，请参阅[定价页面](https://www.azure.cn/pricing/details/cosmos-db/)。

<!--Not Available on ## Billing with Azure Cosmos DB reserved capacity-->

## <a name="next-steps"></a>后续步骤

接下来，可通过以下文章了解 Azure Cosmos DB 中的成本优化：

* 详细了解 [Cosmos DB 定价模型如何对客户而言更具经济效益](total-cost-ownership.md)
* 详细了解[开发和测试优化](optimize-dev-test.md)
* 详细了解如何[优化吞吐量成本](optimize-cost-throughput.md)
* 详细了解如何[优化存储成本](optimize-cost-storage.md)
* 详细了解如何[优化读取和写入成本](optimize-cost-reads-writes.md)
* 详细了解如何[优化查询成本](optimize-cost-queries.md)
* 详细了解[优化多区域 Azure Cosmos 帐户的成本](optimize-cost-regions.md)

<!--Update_Description: wording update -->