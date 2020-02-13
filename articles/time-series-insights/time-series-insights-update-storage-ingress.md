---
title: 预览版中的数据存储和引入 - Azure 时序见解 | Microsoft Docs
description: 了解 Azure 时序见解预览版中的数据存储和引入。
author: lyrana
ms.author: v-yiso
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
origin.date: 12/31/2019
ms.date: 02/17/2020
ms.custom: seodec18
ms.openlocfilehash: 214cfd6618ce62d569f35a300e38873d0d8d893a
ms.sourcegitcommit: 925c2a0f6c9193c67046b0e67628d15eec5205c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77068053"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Azure 时序见解预览版中的数据存储和入口

本文介绍 Azure 时序见解预览版中对数据存储和入口的更新， 其中包括底层存储结构、文件格式和时序 ID 属性。 本文还介绍了底层引入过程、最佳做法和当前的预览版限制。

## <a name="data-ingress"></a>数据入口

Azure 时序见解环境包含一个用于收集、处理和存储时序数据的引入引擎。 规划环境时，需要考虑一些事项，以确保处理所有传入的数据，实现较高的引入规模，并最大限度地降低引入延迟（TSI 读取和处理事件源中的数据所花费的时间）。 在 Azure 时序见解预览版中，数据引入策略决定了数据可以源自何处，以及应采用哪种格式。

### <a name="ingress-policies"></a>流入策略

#### <a name="event-sources"></a>事件源

时序见解预览版支持以下事件源：

- [Azure IoT 中心](../iot-hub/about-iot-hub.md)
- [Azure 事件中心](../event-hubs/event-hubs-about.md)

时序见解预览版对每个实例最多支持两个事件源。

> [!WARNING] 
> * 将事件源附加到预览版环境时，可能会遇到较高的初始延迟。 
> 事件源的延迟取决于 IoT 中心或事件中心内当前的事件数。
> * 首次引入事件源数据后，较高的延迟预期将会下降。 如果你持续遇到较高的延迟，请通过 Azure 门户提交支持票证来联系我们。

#### <a name="supported-data-format-and-types"></a>支持的数据格式和类型

Azure 时序见解支持通过 Azure IoT 中心或 Azure 事件中心提交的 UTF8 编码 JSON。 

下面是支持的数据类型列表。

| 数据类型 | 说明 |
|-----------|------------------|-------------|
| bool      |   具有两种状态之一的数据类型：true 或 false。       |
| dateTime    |   表示某个时刻，通常以日期和当天的时间表示。 DateTime 应采用 ISO 8601 格式。      |
| Double    |   双精度 64 位 IEEE 754 浮点数
| string    |   由 Unicode 字符组成的文本值。          |

#### <a name="objects-and-arrays"></a>对象和数组

可将复杂类型（例如对象和数组）作为事件有效负载的一部分发送，但存储的数据将会经历一个平展过程。 若要详细了解如何调整 JSON 事件以及有关复杂类型和嵌套对象平展的详细信息，请参阅有关[如何调整要引入和查询的 JSON](./time-series-insights-update-how-to-shape-events.md) 的页。


### <a name="ingress-best-practices"></a>有关引入的最佳做法

建议采用以下最佳做法：

* 在同一区域中配置时序见解和 IoT 中心或事件中心，以降低网络问题造成的引入延迟。
* 通过计算预计引入速率并检查计算结果是否处于下面所列的受支持速率范围内，来规划规模需求
* 阅读[如何为引入和查询调整 JSON](./time-series-insights-update-how-to-shape-events.md) 来了解如何优化和调整 JSON 数据，以及当前的预览版限制。

### <a name="ingress-scale-and-limitations-in-preview"></a>预览版中的引入规模和限制


#### <a name="per-environment-limitations"></a>每个环境的限制

一般而言，引入速率被视为与组织中的设备数、事件发出频率以及每个事件的大小因素有关：

*  **设备数** × **事件发出频率** × **每个事件的大小**。

默认情况下，对于 **每个 TSI 环境**，时序见解预览版可按每秒最多 1 兆字节 (MBps) 的速率引入传入的数据。 如果这还不能满足你的要求，请通过在 Azure 门户中提交支持票证联系我们，我们可为环境支持最高 16 MBps 的速率。
 
示例 1：Contoso Shipping 有 100,000 台设备，它们每分钟发出某个事件三次。 事件大小为 200 字节。 Contoso Shipping 使用包含 4 个分区的事件中心作为 TSI 事件源。
其 TSI 环境的引入速率为：100,000 个设备 * 200 字节/事件 * (每秒 3 个事件/60) = 1 MBps。
每个分区的引入速率为 0.25 MBps。
Contoso Shipping 的引入率在预览版缩放限制范围内。
 
示例 2：Contoso Fleet Analytics 有 60,000 台设备，它们每秒发出某个事件。 Contoso Fleet Analytics 使用 IoT 中心（包含 24 个分区，4 个一组）作为 TSI 事件源。 事件大小为 200 字节。
环境引入速率为：20,000 个设备 * 200 字节/事件 * (每秒 1 个事件) = 4 MBps。
每个分区的速率为 1 MBps。
Contoso Fleet Analytics 需要通过 Azure 门户向 TSI 提交请求，以提供一个专用环境来实现此规模。

#### <a name="hub-partitions-and-per-partition-limits"></a>中心分区和每个分区的限制

规划 TSI 环境时，必须考虑要连接到 TSI 的事件源的配置。 Azure IoT 中心和事件中心都利用分区来实现事件处理的水平缩放。  分区是中心内保留的有序事件。 分区计数是在 IoT 或事件中心创建阶段设置的，且不可更改。 有关确定分区计数的详细信息，请参阅事件中心常见问题解答“我需要多少分区？” 对于使用 IoT 中心的 TSI 环境，大多数 IoT 中心一般只需要 4 个分区。 无论是为 TSI 环境创建新的中心还是使用现有的中心，都需要计算每个分区的引入速率，以确定它是否在预览版的限制范围内。 在 TSI 预览版中，**每个分区**的限制目前为 0.5 MB/秒。 参考以下示例。如果你是 IoT 中心用户，请注意以下特定于 IoT 中心的注意事项。

#### <a name="iot-hub-specific-considerations"></a>特定于 IoT 中心的注意事项

在 IoT 中心内创建设备时，会将该设备分配到某个分区，而分区分配不会更改。 这样，IoT 中心就可以保证事件的排序。 但是，在某些方案中，这会影响到用作下游读取器的 TSI。 使用相同的网关设备 ID 将来自多个设备的消息转发到中心时，这些消息将抵达同一分区，因此可能会超出每个分区的规模限制。 

**影响**：如果单个分区的引入速率持续超过预览版的限制，则 TSI 读取器有可能在超出 IoT 中心数据保留期之前永远赶不上进度。 这会导致数据丢失。

建议如下： 

* 在部署解决方案之前按环境和按分区引入速率进行计算
* 确保以最大可能的程度对 IoT 中心设备（因而也包括分区）进行负载均衡

> [!WARNING]
> 对于使用 IoT 中心作为事件源的环境，请使用正在使用的中心设备数计算引入速率，以确保速率低于预览版中每个分区 0.5 MBps 的限制。

  ![IoT 中心分区示意图](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

有关吞吐量单位和分区的详细信息，请参阅以下链接：


* [IoT 中心规模](/iot-hub/iot-hub-scaling)
* [事件中心规模](/event-hubs/event-hubs-scalability#throughput-units)
* [事件中心分区](/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>数据存储

创建时序见解预览版即用即付 SKU 环境时，请创建两个 Azure 资源：

* 可以选择性地包含热存储功能的时序见解预览版环境。
* 用于存储冷数据的 Azure 存储常规用途 V1 Blob 帐户。

只能通过[时序查询](./time-series-insights-update-tsq.md)使用热存储中的数据。 

时序见解预览版以 [Parquet 文件格式](#parquet-file-format-and-folder-structure)将冷存储数据保存到 Azure Blob 存储中。 时序见解预览版以独占方式管理此冷存储数据，但允许将这些数据作为标准 Parquet 文件直接进行读取。

> [!WARNING]
> 冷存储数据所在 Azure Blob 存储帐户的所有者对该帐户中的所有数据拥有完全访问权限。 此访问权限包括“写入”和“删除”权限。 请不要编辑或删除时序见解预览版写入的数据，否则可能导致数据丢失。

### <a name="data-availability"></a>数据可用性

为了实现最佳查询性能，时序见解预览版会将数据分区并为其编制索引。 数据在编制索引后可供查询。 正在引入的数据量可能会影响这种可用性。

> [!IMPORTANT]
> 在预览版中，可能需要等待最长 60 秒才能看到数据。 如果遇到 60 秒以上的较高延迟，请通过 Azure 门户提交支持票证。

## <a name="azure-storage"></a>Azure 存储

本部分介绍与 Azure 时序见解预览版相关的 Azure 存储详细信息。

有关 Azure Blob 存储的全面介绍，请阅读[存储 Blob 简介](../storage/blobs/storage-blobs-introduction.md)。

### <a name="your-storage-account"></a>存储帐户

创建时序见解预览版即用即付环境时，会创建一个 Azure 存储常规用途 V1 Blob 帐户作为长期冷存储。  

时序见解预览版在 Azure 存储帐户中发布每个事件的最多两个副本。 初始副本包含按引入时间排序的事件，并且该副本始终会保留，因此你可以使用其他服务来访问它。 可以使用 Spark、Hadoop 和其他熟悉的工具来处理原始 Parquet 文件。 

时序见解预览版可将 Parquet 文件重新分区，以针对时序见解查询进行优化。 此重新分区的数据副本也会保存。

在公共预览版中，数据无限期存储在 Azure 存储帐户中。

#### <a name="writing-and-editing-time-series-insights-blobs"></a>编写和编辑时序见解 Blob

为了确保查询性能和数据可用性，请不要编辑或删除时序见解预览版创建的任何 Blob。

#### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>从时序见解预览版访问和导出数据

你可能想要访问时序见解预览版资源管理器中显示的数据，以便与其他服务结合使用这些数据。 例如，可以使用数据在 Power BI 中生成报表，或使用 Azure 机器学习工作室训练机器学习模型。 或者，可以在 Jupyter Notebook 中使用数据进行转换、可视化和建模。

可通过三种常规方式访问数据：

* 使用“获取事件”查询通过时序见解预览版 API 访问。 有关此 API 的详细信息，请参阅[时序查询](./time-series-insights-update-tsq.md)。
* 从 Azure 存储帐户直接访问。 需要对用于访问时序见解预览版数据的任何帐户拥有读取访问权限。 有关详细信息，请参阅[管理对存储帐户资源的访问权限](../storage/blobs/storage-manage-access-to-resources.md)。

#### <a name="data-deletion"></a>数据删除

请不要删除时序见解预览版文件。 请只在时序见解预览版内部管理相关数据。

### <a name="parquet-file-format-and-folder-structure"></a>Parquet 文件格式和文件夹结构

Parquet 是一种开源的列式文件格式，旨在提高存储效率和性能。 出于这些原因，时序见解预览版使用 Parquet。 它按时序 ID 将数据分区，以提高大规模查询的性能。  

有关 Parquet 文件类型的详细信息，请参阅 [Parquet 文档](https://parquet.apache.org/documentation/latest/)。

时序见解预览版按如下方式存储数据的副本：

* 首先，初始副本按引入时间分区，并大致按抵达顺序存储数据。 数据驻留在 `PT=Time` 文件夹中：

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* 其次，重新分区的副本按时序 ID 的分组分区，驻留在 `PT=TsId` 文件夹中：

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

在这两种情况下，时间值对应于 Blob 创建时间。 将保留 `PT=Time` 文件夹中的数据。 `PT=TsId` 文件夹中的数据将不断针对查询进行优化，而不会保持静态。

> [!NOTE]
> * `<YYYY>` 映射到四位数的年份表示形式。
> * `<MM>` 映射到两位数的月份表示形式。
> * `<YYYYMMDDHHMMSSfff>` 映射到时间戳表示形式，其中包括四位数的年份 (`YYYY`)、两位数的月份 (`MM`)、两位数的日 (`DD`)、两位数的小时 (`HH`)、两位数的分钟 (`MM`)、两位数的秒 (`SS`) 和三位数的毫秒 (`fff`)。

时序见解预览版事件按如下所示映射到 Parquet 文件内容：

* 每个事件映射到单个行。
* 每一行包含带有事件时间戳的**时间戳**列。 时间戳属性永远不会为 null。 如果未在事件源中指定时间戳属性，该属性默认为**事件排队时间**。 时间戳始终为 UTC 时间。
* 每一行包含创建时序见解环境时定义的时序 ID 列。 属性名称包含 `_string` 后缀。
* 作为遥测数据发送的所有其他属性映射到以 `_string`（字符串）、`_bool`（布尔值）、`_datetime`（日期时间）或 `_double`（双精度值）结尾的列名称，具体取决于属性类型。
* 此映射方案适用于文件格式的第一个版本（以 **V=1** 表示）。 随着此功能的不断演进，名称可能会递增。

## <a name="next-steps"></a>后续步骤

- 阅读[如何针对传入和查询调整 JSON](./time-series-insights-update-how-to-shape-events.md)。

- 了解新型[数据建模](./time-series-insights-update-tsm.md)。
