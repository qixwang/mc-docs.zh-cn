---
title: 如何在 Azure Cosmos DB 中选择正确的吞吐量套餐
description: 了解如何针对你的工作负荷在标准（手动）预配吞吐量与自动缩放预配吞吐量之间进行选择。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/19/2020
ms.date: 06/01/2020
ms.author: v-yeche
ms.openlocfilehash: b7ad7f56b5aced71bac8332ee37f652ac172f925
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200240"
---
<!--Verified successfully-->
# <a name="how-to-choose-between-standard-manual-and-autoscale-provisioned-throughput"></a>如何在标准（手动）预配吞吐量与自动缩放预配吞吐量之间进行选择 

Azure Cosmos DB 支持两种预配吞吐量类型或套餐：标准（手动）和自动缩放。 这两种吞吐量类型都适用于对性能和缩放要求很高的关键工作负荷，并且在吞吐量、可用性、延迟和一致性方面由相同的 Azure Cosmos DB SLA 来提供保证。

本文介绍了如何针对你的工作负荷在标准（手动）预配吞吐量与自动缩放预配吞吐量之间进行选择。 

## <a name="overview-of-provisioned-throughput-types"></a>预配吞吐量类型概述
在深入探讨标准（手动）与自动缩放之间的差异之前，首先应了解预配吞吐量在 Azure Cosmos DB 中的工作原理。 

使用预配吞吐量时，你将设置工作负荷所需的吞吐量，以每秒请求单位数（RU/秒）进行度量。 该服务预配支持吞吐量要求所需的容量。 针对服务的数据库操作（例如读取、写入和查询）占用一定数量的请求单位 (RU)。 详细了解[请求单位](request-units.md)。

下表显示了标准（手动）与自动缩放之间的大概比较。

|说明|标准（手动）|自动缩放|
|-------------|------|-------|
|最适用于|流量稳定或可预测的工作负荷|流量可变或不可预测的工作负荷。 请参阅[自动缩放的用例](provision-throughput-autoscale.md#use-cases-of-autoscale)。|
|工作原理|你预配不随时间变化的设定数量的 RU/秒 `T`，除非手动更改它们。 每秒，你可以使用最高 `T` RU/秒的吞吐量。 <br/><br/>例如，如果设置了标准（手动）400 RU/秒，则吞吐量将保持在 400 RU/秒。|你设置不希望系统超出的最高或最大 RU/秒 `Tmax`。 系统自动对吞吐量 `T` 进行缩放，使 `0.1* Tmax <= T <= Tmax`。 <br/><br/>例如，如果你将自动缩放最大 RU/秒设置为 4000 RU/秒，则系统将在 400 - 4000 RU/秒之间进行缩放。|
|何时使用此功能|你希望自己手动管理吞吐量容量（RU/秒）和缩放。<br/><br/>你的预配 RU/ 秒的利用率一直比较高。 在一个月的所有时间中，如果你设置了预配 RU/秒 `T`，并且在 66% 或更多时间内全额使用了该容量，则估计你将使用标准（手动）预配 RU/秒来节省容量。<br/><br/>这基于标准（手动）中的设置 `T` 与自动缩放中的相同数量 `Tmax` 之间的比较。 |你希望 Azure Cosmos DB 根据使用情况来管理吞吐量容量（RU/秒）和缩放。<br/><br/>你的 RU/秒使用量可变或难以预测。 在一个月的所有时间中，如果你将自动缩放最大 RU/秒设置为 `Tmax`，并且在 66% 或更少的时间内使用了全额 `Tmax`，则估计你将使用自动缩放来节省容量。<br/><br/>这基于自动缩放 `Tmax` 设置与标准（手动）吞吐量中的相同数量 `T` 之间的比较。|
|计费模式|无论消耗了多少 RU，都是针对预配 RU/秒按小时计费。<br/><br/>示例： <li>预配 400 RU/秒</li><li>第 1 小时：无请求</li><li>第 2 小时：400 RU/秒的请求数</li><br/><br/>对于第 1 小时和第 2 小时，都将以[标准（手动）费率](https://www.azure.cn/pricing/details/cosmos-db/)按 400 RU/秒计费。|计费按小时计算，并且按系统在该小时内缩放到的最高 RU/秒计算。 <br/><br/>示例： <li>将自动缩放最大 RU/秒预配为 4000 RU/秒（在 400 - 4000 RU/秒之间缩放）</li><li>第 1 小时：系统纵向缩放到最高值 3500 RU/秒</li><li>第 2 小时：由于没有使用量，系统纵向缩减到最小值 400 RU/秒（始终为 `Tmax` 的 10%）</li><br/><br/>将按照[自动缩放预配吞吐量费率](https://www.azure.cn/pricing/details/cosmos-db/)进行计费，第 1 小时按 3500 RU/秒计费，第 2 小时按 400 RU/秒计费。 每 RU/秒的自动缩放费率为 1.5 *标准（手动）费率。
|如果超出了预配 RU/秒，会发生什么情况|RU/秒保持为预配值不变。 任何在一秒内消耗超过预配 RU 的请求都将被实施速率限制，并且响应会建议你等待一段时间后重试。 如果需要，你可以手动增大或减小 RU/秒。| 系统会将 RU/秒纵向扩展到自动缩放的最大 RU/秒。 任何在一秒内消耗超过自动缩放的最大 RU/秒的请求都将被实施速率限制，并且响应会建议你等待一段时间后重试。|

## <a name="understand-your-traffic-patterns"></a>了解你的流量模式

### <a name="new-applications"></a>新应用程序 ###
如果你正在构建新的应用程序，并且尚不知道你的流量模式，则你可能想要从入口点 RU/秒（或最小 RU/秒）开始，以避免在开始时过度预配。 或者，如果你有缩放需求不高的小型应用程序，则你可能希望只预配最小的入口点 RU/秒以优化成本。 在这两种情况下，标准（手动）和自动缩放都合适。 下面是应考虑的事项：

如果你将标准（手动）RU/秒预配为入口点 400 RU/秒，则你不能消耗 400 RU/秒以上，除非你手动更改吞吐量。 每小时将以标准（手动）预配吞吐量费率按 400 RU/秒计费。

如果你将自动缩放吞吐量的入口点最大 RU/秒预配为 4000 RU/秒，则资源将在 400 到 4000 RU/秒之间缩放。 由于每 RU/秒的自动缩放吞吐量计费费率是标准（手动）费率的 1.5 倍，对于系统已缩减到最低 400 RU/秒的时段，你的账单费用将高于手动预配的 400 RU/秒的账单。 但在使用自动缩放时，在任何时候，如果应用程序流量达到高峰，你都可以消耗最多 4000 RU/秒，无需用户执行任何操作。 通常情况下，对于自动缩放，应将任何时候都能够消耗最大 RU/秒的好处与 1.5 倍的费率进行权衡。

<!--Not Available on [capacity calculator](estimate-ru-with-capacity-planner.md)-->

### <a name="existing-applications"></a>现有应用程序 ###

如果你有使用标准（手动）预配吞吐量的现有应用程序，则可以使用 Azure Monitor 指标来确定你的流量模式是否适合自动缩放。 

<!--Not Avaialble on [Azure Monitor metrics](../azure-monitor/insights/cosmosdb-insights-overview.md)-->

首先，找到你的数据库或容器的规范化请求单位消耗指标。 规范化利用率度量当前对标准（手动）预配吞吐量的使用程度。 该数字越接近 100%，说明你使用预配 RU/秒越充分。

<!--Not Available on  [Learn more](monitor-normalized-request-units.md#view-the-normalized-request-unit-consumption-metric) about the metric.-->

<!--Not Available on [normalized request unit consumption metric](monitor-normalized-request-units.md#view-the-normalized-request-unit-consumption-metric)-->

接下来，确定规范化利用率如何随时间变化。 如果你发现规范化利用率可变或不可预测，请考虑对数据库或容器启用自动缩放。 反之，如果它稳定且可预测，则考虑保持使用标准（手动）预配吞吐量。 

> [!TIP]
> 使用标准（手动）吞吐量，你可以使用规范化利用率指标来估计在切换到自动缩放时可能使用的实际 RU/秒。 将某个时间点的规范化利用率与当前预配的标准（手动）RU/秒相乘。 例如，如果你预配了 5000 RU/秒，并且规范化利用率是 90%，则 RU/秒使用率为 0.9 * 5000 = 4500 RU/秒。 如果你发现你的流量模式可变，但你预配过度或不足，则可能需要启用自动缩放，然后相应地更改自动缩放最大 RU/秒设置。

## <a name="measure-and-monitor-your-usage"></a>度量和监视使用情况
选择吞吐量类型后，随着时间的推移，你应当监视应用程序并根据需要进行调整。 

使用自动缩放时，请使用 Azure Monitor 查看预配的自动缩放最大 RU/秒（**自动缩放最大吞吐量**）和系统当前缩放到的 RU/秒（**预配吞吐量**）。 下面是使用自动缩放的可变或不可预测的工作负荷的示例。 注意，当没有任何流量时，系统会将 RU/秒缩放到最小值（即最大 RU/秒的 10%），在本例中分别为 5000 RU/秒和 50000 RU/秒。 

![使用自动缩放的工作负荷的示例](media/how-to-choose-offer/autoscale-metrics-azure-monitor.png)

> [!NOTE]
> 使用标准（手动）预配吞吐量时，**预配吞吐量**指标指的是你作为用户设置的值。 使用自动缩放吞吐量时，此指标指的是系统当前缩放到的 RU/秒。

## <a name="next-steps"></a>后续步骤
* 使用 [RU 计算器](https://cosmos.azure.com/capacitycalculator/)估算新工作负荷的吞吐量。
* 使用 [Azure Monitor](monitor-cosmos-db.md#view-operation-level-metrics-for-azure-cosmos-db) 监视现有的工作负荷。
* 了解如何[在 Azure Cosmos 数据库或容器上预配自动缩放吞吐量](how-to-provision-autoscale-throughput.md)。
* 查看[自动缩放常见问题解答](autoscale-faq.md)。

<!-- Update_Description: new article about how to choose offer -->
<!--NEW.date: 06/01/2020-->