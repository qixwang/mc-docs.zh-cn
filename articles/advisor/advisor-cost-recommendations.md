---
title: 使用 Azure 顾问降低服务成本
description: 使用 Azure 顾问优化 Azure 部署的成本。
ms.topic: article
origin.date: 01/29/2019
ms.date: 07/29/2020
ms.author: lingliw
ms.openlocfilehash: a78f695ee505d4e574ad3556908eb868fe8b487b
ms.sourcegitcommit: b5794af488a336d84ee586965dabd6f45fd5ec6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87508331"
---
# <a name="reduce-service-costs-using-azure-advisor"></a>使用 Azure 顾问降低服务成本

通过识别闲置和未充分利用的资源，顾问有助于优化和降低 Azure 总支出。 可在顾问仪表板的“成本”选项卡获取成本建议。

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>通过调整或关闭未充分利用的实例来优化虚拟机花费 

虽然某些应用程序方案有意使虚拟机利用率较低，但通过管理虚拟机大小和数量通常可降低成本。 如果过去 7 天内 CPU 使用率的最大值不到 3%，网络使用率的最大值不到 2%（在 95% 的情况下），则顾问的高级评估模型会考虑关闭虚拟机。 如果可以在较小的 SKU 中（在同一 SKU 系列中）适应当前负荷，或者可以在减少实例数的情况下这样做，使得当前的负荷在不是面向用户的工作负荷的情况下不超出 80% 的使用率，在是面向用户的工作负荷的情况下不超出 40% 的使用率，则可认为虚拟机的大小合适。 在这里，可以通过分析工作负荷的 CPU 使用率特征来确定工作负荷的类型。

建议的操作是关机或重设大小，具体取决于这种情况下建议使用的资源。 顾问显示了与建议的操作（重设大小或关机）相对应的成本节省估算值。 另外，对于重设大小这种建议的操作，顾问提供了当前 SKU 信息和目标 SKU 信息。 

若要加强对低使用率虚拟机的标识，可在每个订阅的基础上调整 CPU 使用率。

## <a name="optimize-spend-for-mariadb-mysql-and-postgresql-servers-by-right-sizing"></a>通过适当调整大小优化 MariaDB、MySQL 和 PostgreSQL 服务器的支出 
若 MariaDB/MySQL/PostgreSQL 数据库服务器资源在过去 7 天内长时间未被充分利用，顾问将分析使用情况并提出建议。 资源利用率低会导致不必要的支出，这个问题可以在不显著影响性能的情况下得到修复。 为了降低成本并高效管理资源，我们建议将计算大小 (vCore) 减少一半。

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>通过消除未设置的 ExpressRoute 线路来降低成本

顾问将识别提供程序状态为“未设置”长达一个月以上的 ExpressRoute 线路将被顾问标识，如果没有使用连接性提供程序配置该线路的计划，顾问将建议删除它。

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>通过删除或重新配置空闲虚拟网络网关来降低成本

顾问会识别已闲置超过 90 天的虚拟网关。 由于这些网关按小时计费，因此如果不打算再使用它们，则应考虑重新配置或删除它们。 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>购买虚拟机预留实例可节省即用即付成本

顾问将查看过去 30 天的虚拟机使用情况，并确定是否可以通过购买 Azure 预留来节省资金。 顾问将展示可能在其中最大程度节省资金的区域和大小，并展示通过购买预留节约下来的估算费用。 

顾问还会向你通知在接下来的 30 天内将过期的预留实例。 它将建议你购买新的预留实例来避免即用即付定价。

## <a name="buy-reserved-instances-for-several-resource-types-to-save-over-your-pay-for-advance-costs"></a>购买多个资源类型的预留实例，以节省预付费成本

我们分析了过去 30 天内以下资源列表的使用模式，并推荐可以最大限度节省成本的预留容量购买方式。 
### <a name="cosmos-db-reserved-capacity"></a>Cosmos DB 保留容量
我们分析了过去 30 天的 Cosmos DB 使用模式，并建议购买保留容量以优化成本。 使用预留容量，你可以按小时预购买 Cosmos DB，从而节省预付费成本。 预留容量是一项计费权益，它将自动应用于新部署或现有部署。 基于 3 年期预留定价并根据过去 30 天内观察到的使用模式进行推测，从而计算各订阅成本节省的预估值。 通过购买预留可以获得共享范围建议，并且可以进一步增加节省额。

### <a name="sql-paas-reserved-capacity"></a>SQL PaaS 预留容量
我们分析了过去 30 天内的 SQL PaaS 弹性池和托管实例使用模式，建议你购买预留容量，以最大限度地节省成本。 通过使用预留容量，可以预购买 SQL DB 按小时计的使用量，从而节省 SQL 计算成本。 SQL 许可证单独收费，不会因预留而打折。 预留容量是一项计费权益，它将自动应用于新部署或现有部署。 基于 3 年期预留定价并根据过去 30 天内观察到的使用模式进行推测，从而计算各订阅成本节省的预估值。 通过购买预留可以获得共享范围建议，并且可以进一步提高成本节省。

### <a name="app-service-stamp-fee-reserved-capacity"></a>应用服务印花费预留容量
我们会分析你在过去 30 天内的应用服务独立环境印花费使用模式，建议你购买可最大限度地节省成本的预留容量。 通过预留容量，可以为独立环境印花费预购买按小时计的使用量，比预付费方式节省更多成本。 请注意，预留容量仅适用于印花费，不适用于应用服务实例。 预留容量是一项计费权益，它将自动应用于新部署或现有部署。 基于 3 年期预留定价并根据过去 30 天内的使用模式，计算各订阅成本节省的预估值。

### <a name="blob-storage-reserved-capacity"></a>Blob 存储预留容量
我们分析了你在过去 30 天内的 Azure Blob 存储使用情况，并计算出了可最大限度节省成本的预留容量购买。 通过预留容量，可以预购买按小时计的使用量，比当前的按需方式节省更多成本。 Blob 存储预留容量仅适用于存储在 Azure Blob (GPv2)和 Azure Data Lake Storage (Gen 2) 上的数据。 预留容量是一项计费权益，它将自动应用于新部署或现有部署。 基于 3 年期预留定价并根据过去 30 天内观察到的使用模式，计算各订阅成本节省的预估值。 通过购买预留可以获得共享范围建议，并且可以进一步提高成本节省。

### <a name="mariadb-mysql-and-postgresql-reserved-capacity"></a>MariaDB、MySQL 和 PostgreSQL 预留容量
我们会分析你在过去 30 天内的 Azure Database for MariaDB、Azure Database for MySQL 和 Azure Database for PostgreSQL 使用模式，建议你购买可最大限度地节省成本的预留容量。 通过预留容量，可以预购买按小时计的 MariaDB、MySQL 和 PostgreSQL 使用量，节省更多成本。 预留容量是一项计费权益，它将自动应用于新部署或现有部署。 基于 3 年期预留定价并根据过去 30 天内的使用模式，计算各订阅成本节省的预估值。 通过购买预留可以获得共享范围建议，并且可以进一步提高成本节省。

### <a name="synapse-analytics-formerly-sql-dw-reserved-capacity"></a>Synapse Analytics（以前称为 SQL DW）预留容量
我们会分析你在过去 30 天内的 Azure Synapse Analytics 使用模式，建议你购买可以最大限度节省成本的预留容量。 通过使用预留容量，可以预购买 Synapse Analytics 按小时计的使用量，与按需成本相比节省更多成本。 预留容量是一项计费权益，它将自动应用于新部署或现有部署。 基于 3 年期预留定价并根据过去 30 天内观察到的使用模式，计算各订阅成本节省的预估值。 通过购买预留可以获得共享范围建议，并且可以进一步提高成本节省。

## <a name="delete-unassociated-public-ip-addresses-to-save-money"></a>删除未关联的公共 IP 地址可节省资金

顾问可以标识目前未关联到 Azure 资源（例如负载均衡器或 VM）的公共 IP 地址。 这些公共 IP 地址会产生少许费用。 如果不打算使用它们，删除它们可以节省成本。

## <a name="delete-azure-data-factory-pipelines-that-are-failing"></a>删除将要发生故障的 Azure 数据工厂管道

Azure 顾问会检测到反复发生故障的 Azure 数据工厂管道，并建议你解决问题，或者在不需要这些故障管道的情况下将其删除。 即使这些管道在发生故障时未为你提供服务，我们也会向你收取相关费用。 

## <a name="create-an-ephemeral-os-disk-recommendation"></a>创建临时 OS 磁盘的建议
使用[临时 OS 磁盘](https://docs.microsoft.com/azure/virtual-machines/windows/ephemeral-os-disks)，客户可获得以下好处：节省 OS 磁盘的存储成本。 降低 OS 磁盘的读取/写入延迟。 通过将 OS（和临时磁盘）重置为其原始状态，加速 VM 重置映像操作。 对于生存期较短的 IaaS VM 或具有无状态工作负载的 VM，最好使用临时 OS 磁盘。 顾问对可利用临时 OS 磁盘的资源提出了建议。 


## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>如何访问 Azure 顾问中的成本建议

1. 登录到 [Azure 门户](https://portal.azure.cn)。

1. 在任意页面中搜索并选择[顾问](https://portal.azure.cn/#blade/Microsoft_Azure_Expert/AdvisorBlade)。

1. 在顾问仪表板中，选择“成本”选项卡 。

## <a name="next-steps"></a>后续步骤

若要了解有关顾问建议的详细信息，请参阅以下资源：
* [顾问简介](advisor-overview.md)
* [顾问入门](advisor-get-started.md)
* [顾问性能建议](advisor-performance-recommendations.md)
* [顾问高可用性建议](advisor-high-availability-recommendations.md)
* [顾问安全性建议](advisor-security-recommendations.md)
* [顾问卓越运营建议](advisor-operational-excellence-recommendations.md)

