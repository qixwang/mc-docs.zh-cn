---
title: 使用流式引入将数据引入 Azure 数据资源管理器
description: 了解如何使用流式引入将数据引入（载入）Azure 数据资源管理器。
author: orspod
ms.author: v-tawe
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
origin.date: 08/30/2019
ms.date: 12/16/2019
ms.openlocfilehash: be0024ce68ed048a3e3bc566e35f8c0d4caf6a89
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336346"
---
# <a name="streaming-ingestion-preview"></a>流式引入（预览版）

流式引入适用于需要低延迟，且各种量级数据的引入时间小于 10 秒的方案。 对于要将相对较小的数据流式传输到表中（每秒传输几条记录），但总体数据引入量较高（每秒引入数千条记录）的一个或多个数据库，可以使用流式引入来优化其中许多表的操作处理。

当每个表的数据量增大到的每秒 1 MB 以上时，请使用经典（批量）引入而不是流式引入。 请阅读[数据引入概述](/data-explorer/ingest-data-overview)来详细了解各种引入方法。

> [!NOTE]
> 流式引入不支持以下功能：
> * [数据库游标](https://docs.microsoft.com/azure/kusto/management/databasecursor)。
> * [数据映射](https://docs.microsoft.com/azure/kusto/management/mappings)。 仅支持[预先创建](https://docs.microsoft.com/azure/kusto/management/tables#create-ingestion-mapping)的数据映射。 

## <a name="prerequisites"></a>先决条件

* 如果没有 Azure 订阅，请在开始前创建一个[试用 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial/)。
* 登录到 [Web UI](https://dataexplorer.azure.cn/)。
* 创建 [Azure 数据资源管理器群集和数据库](create-cluster-database-portal.md)

## <a name="enable-streaming-ingestion-on-your-cluster"></a>在群集上启用流式引入

1. 在 Azure 门户中，转到 Azure 数据资源管理器群集。 在“设置”中选择“配置”。   
1. 在“配置”窗格中，选择“打开”以启用“流式引入”。   
1. 选择“保存”  。
 
    ![打开流式引入](media/ingest-data-streaming/streaming-ingestion-on.png)
 
1. 在 [Web UI](https://dataexplorer.azure.cn/) 中，针对要接收流数据的表或数据库定义[流引入策略](https://docs.microsoft.com/azure/kusto/concepts/streamingingestionpolicy)。 

    > [!NOTE]
    > * 如果在数据库级别定义了该策略，将为该数据库中的所有表启用流式引入。
    > * 应用的策略只能引用新引入的数据，而不能引用数据库中的其他表。

## <a name="use-streaming-ingestion-to-ingest-data-to-your-cluster"></a>使用流式引入将数据引入到群集

支持两种流式引入类型：

* 用作数据源的[事件中心](/data-explorer/ingest-data-event-hub)
* 自定义引入需要编写使用某个 Azure 数据资源管理器客户端库的应用程序。 有关示例应用程序，请参阅[流式引入示例](https://github.com/Azure/azure-kusto-samples-dotnet/tree/master/client/StreamingIngestionSample)。

### <a name="choose-the-appropriate-streaming-ingestion-type"></a>选择适当的流式引入类型

|   |事件中心  |自定义引入  |
|---------|---------|---------|
|启动引入之后、数据可供查询之前的数据延迟   |    延迟更长     |   延迟更短      |
|开发开销    |   快速轻松的设置，不产生开发开销    |   应用程序需要处理错误并确保数据一致性，因此开发开销较高     |

## <a name="disable-streaming-ingestion-on-your-cluster"></a>在群集上禁用流式引入

> [!WARNING]
> 禁用流式引入可能需要几个小时。

1. 从所有相关表和数据库中删除[流式引入策略](https://docs.microsoft.com/azure/kusto/concepts/streamingingestionpolicy)。 删除流式引入策略会触发将流式引入数据从初始存储移到列存储中的永久存储（盘区或分片）的行为。 数据移动的持续时间从几秒钟到几小时不等，具体取决于初始存储中的数据量，以及群集的 CPU 和内存利用率。
1. 在 Azure 门户中，转到 Azure 数据资源管理器群集。 在“设置”中选择“配置”。   
1. 在“配置”窗格中，选择“关闭”以禁用“流式引入”。   
1. 选择“保存”  。

    ![关闭流式引入](media/ingest-data-streaming/streaming-ingestion-off.png)

## <a name="limitations"></a>限制

* 增大 VM 和群集大小可以提高流式引入的性能和容量。 并发引入限制为每个内核 6 个引入。 例如，对于 16 核 SKU（如 D14 和 L16），支持的最大负载为 96 个并发引入。 对于 2 核 SKU（如 D11），支持的最大负载为 12 个并发引入。
* 每个引入请求的数据大小限制为 4 MB。
* 对流式引入服务进行架构更新（例如创建和修改表与引入映射）最长可能需要花费 5 分钟时间。
* 即使数据不是流式引入的，在群集上启用流式引入也会占用群集计算机的一部分本地 SSD 磁盘用于存储流式引入数据，因此会减少热缓存的可用存储。

## <a name="next-steps"></a>后续步骤

* [在 Azure 数据资源管理器中查询数据](web-query-data.md)
