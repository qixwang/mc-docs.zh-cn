---
title: 了解 Azure 流分析的输出
description: 本文介绍 Azure 流分析提供的数据输出选项，包括用于分析结果的 Power BI。
author: Johnnytechn
ms.author: v-johya
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/23/2020
ms.openlocfilehash: 0a5b1fdc5bf522c10935f9e3c00d123a891c0d94
ms.sourcegitcommit: 1c01c98a2a42a7555d756569101a85e3245732fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85097157"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>了解 Azure 流分析的输出

本文介绍适用于 Azure 流分析作业的输出类型。 输出可帮助存储和保存流分析作业的结果。 使用输出数据，可进一步进行业务分析和数据的数据仓储。

设计流分析查询时，使用 [INTO 子句](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics)引用输出的名称。 可针对每个作业使用单个输出，如果需要，也可通过在查询中提供多个 INTO 子句，针对每个流式处理作业使用多个输出。

若要创建、编辑和测试流分析作业输出，可使用 [Azure 门户](stream-analytics-quick-create-portal.md#configure-job-output)、[Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job)、[.NET API](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet)、[REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output) 和 [Visual Studio](stream-analytics-quick-create-vs.md)。

<!-- Not Available on [Visual Studio](stream-analytics-quick-create-vs.md)-->

某些输出类型支持[分区](#partitioning)。 [输出批大小](#output-batch-size)可变，这是为了优化吞吐量。
<!-- Not Available ## Azure Data Lake Storage Gen 1-->

## <a name="sql-database"></a>SQL 数据库

可以将 [Azure SQL 数据库](https://docs.azure.cn/zh-cn/sql-database/)用作本质上为关系型数据的输出，也可以将其用于所依赖的内容在关系数据库中托管的应用程序。 流分析作业将写入到 SQL 数据库的现有表中。 表架构必须与作业输出中的字段及其类型完全匹配。 还可以通过 SQL 数据库输出选项将 [Azure SQL 数据仓库](https://docs.azure.cn/zh-cn/sql-data-warehouse/)指定为输出。 若要了解提高写入吞吐量的方法，请参阅[以 Azure SQL 数据库作为输出的流分析](stream-analytics-sql-output-perf.md)一文。

<!--waiting to be moved about https://docs.azure.cn/zh-cn/sql-data-warehouse/ -->
还可以使用 [Azure SQL 数据库托管实例](/sql-database/sql-database-managed-instance)作为输出。 必须[在 Azure SQL 数据库托管实例中配置公共终结点](/sql-database/sql-database-managed-instance-public-endpoint-configure)，然后在 Azure 流分析中手动配置以下设置。 还可以通过手动配置以下设置来支持运行附加了数据库的 SQL Server 的 Azure 虚拟机。

下表列出了属性名称和用于创建 SQL 数据库输出的属性说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |在查询中使用的友好名称，用于将查询输出定向到此数据库。 |
| 数据库 | 将输出发送到的数据库的名称。 |
| 服务器名称 | SQL 数据库服务器名称。 对于 Azure SQL 数据库托管实例，需要指定端口 3342。 例如，“sampleserver.public.database.chinacloudapi.cn,3342” |
| 用户名 | 对数据库拥有写入访问权限的用户名。 流分析仅支持 SQL 身份验证。 |
| 密码 | 用于连接到数据库的密码。 |
| 表 | 将写入输出的表名称。 表名称区分大小写。 此表的架构应与字段数量以及作业输出生成的字段类型完全匹配。 |
|继承分区方案| 一个用于继承先前查询步骤的分区方案，以启用具有多个表的写入器的完全并行拓扑的选项。 有关详细信息，请参阅从 [Azure 流分析输出到 Azure SQL 数据库](stream-analytics-sql-output-perf.md)。|
|最大批数| 每个批量插入事务一起发送的推荐记录数上限。|

## <a name="blob-storage-and-azure-data-lake-gen2"></a>Blob 存储和 Azure Data Lake Gen2

Data Lake Storage Gen2 使 Azure 存储成为在 Azure 上构建企业 Data Lake 的基础。 Data Lake Storage Gen2 从一开始就设计为处理数千万亿字节的信息，同时保持数百千兆位的吞吐量，可让你轻松管理大量数据。Data Lake Storage Gen2 的一个基本特征是在 Blob 存储中添加分层命名空间。

Azure Blob 存储提供了一种经济高效且可扩展的解决方案，用于在云中存储大量非结构化数据。 有关 Blob 存储及其用法的简介，请参阅[使用 Azure 门户上传、下载和列出 blob](../storage/blobs/storage-quickstart-blobs-portal.md)。

下表列出了用于创建 blob 或 ADLS Gen2 输出的属性名称及其说明。

| 属性名称       | 说明                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| 输出别名        | 查询中使用的友好名称，用于将查询输出定向到此 blob 存储。 |
| 存储帐户     | 存储帐户的名称（正在向该存储帐户发送输出）。               |
| 存储帐户密钥 | 与存储帐户关联的密钥。                              |
| 存储容器   | 对存储在 Azure Blob 服务中的 blob 进行逻辑分组。 将 blob 上传到 Blob 服务时，必须为该 blob 指定一个容器。 |
| 路径模式 | 可选。 用于写入指定容器中的 blob 的文件路径模式。 <br /><br /> 在路径模式中，可以选择使用数据时间变量的一个或多个实例指定 blob 写入的频率： <br /> {date}、{time} <br /><br />可以使用自定义 blob 分区从事件数据中指定一个自定义 {field} 名称来对 blob 进行分区。 字段名称是字母数字，并且可以包含空格、连字符和下划线。 对自定义字段的限制包括以下内容： <ul><li>字段名称不区分大小写。 例如，服务无法区分列“ID”和列“id”。</li><li>不允许嵌套字段。 在作业查询中改用别名来“平展”字段。</li><li>不能使用表达式作为字段名称。</li></ul> <br />通过此功能可以在路径中使用自定义日期/时间格式说明符配置。 一次只能指定一个自定义日期和时间格式，并将其用 {datetime:\<specifier>} 关键字括起来。 \<specifier> 允许的输入为 yyyy、MM、M、dd、d、HH、H、mm、m、ss 或 s。 可以在路径中多次使用 {datetime:\<specifier>} 关键字以构成自定义的日期/时间配置。 <br /><br />示例: <ul><li>示例 1：cluster1/logs/{date}/{time}</li><li>示例 2：cluster1/logs/{date}</li><li>示例 3：cluster1/{client_id}/{date}/{time}</li><li>示例 4：cluster1/{datetime:ss}/{myField}，其中查询是：SELECT data.myField AS myField FROM Input;</li><li>示例 5：cluster1/year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}</ul><br />创建的文件夹结构的时间戳遵循 UTC 而不是本地时间。<br /><br />文件命名采用以下约定： <br /><br />{路径前缀模式}/schemaHashcode_Guid_Number.extension<br /><br />示例输出文件：<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> <br />有关此功能的详细信息，请参阅 [Azure 流分析自定义 blob 输出分区](stream-analytics-custom-path-patterns-blob-storage-output.md)。 |
| 日期格式 | 可选。 如果在前缀路径中使用日期令牌，可以选择组织文件所采用的日期格式。 示例：YYYY/MM/DD |
| 时间格式 | 可选。 如果在前缀路径中使用时间令牌，可指定组织文件所采用的时间格式。 目前唯一支持的值是 HH。 |
| 事件序列化格式 | 输出数据的序列化格式。 支持 JSON、CSV、Avro 和 Parquet。 |
|最小行数（仅 Parquet）|每批的最小行数。 对于 Parquet，每个批处理都将创建一个新文件。 当前默认值为 2000 行，允许的最大值为 10000 行。|
|最长时间（仅 Parquet）|每批的最长等待时间。 在此时间后，即使不满足最小行数要求，也会将该批写入输出。 当前默认值为 1 分钟，允许的最大值为 2 小时。 如果 blob 输出具有路径模式频率，则等待时间不能超出分区时间范围。|
| 编码    | 如果使用 CSV 或 JSON 格式，则必须指定一种编码格式。 目前只支持 UTF-8 这种编码格式。 |
| 分隔符   | 仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |
| 格式      | 仅适用于 JSON 序列化。 分隔行指定通过新行分隔各个 JSON 对象，从而格式化输出。 如果选择“行分隔”，则读取 JSON 时，一次读取一个对象。 整个内容本身将不是有效的 JSON。 数组指定输出会被格式化为 JSON 对象的数组。 仅当作业停止或流分析移动到下个时间段时，才关闭此数组。 一般而言，最好使用分隔行 JSON，因为在继续写入输出文件时，无需任何特殊处理。 |

使用 blob 存储作为输出时，在以下情况下 blob 中创建一个新文件：

* 如果文件超出了允许的最大块数（目前为 50,000）。 可达到允许的最大块数，但不能达到允许的最大 blob 大小。 例如，如果输出率很高，则可以看到每个块的字节更多，并且文件大小会更大。 输出率较低时，每个块都有较少的数据，且文件大小较小。
* 如果输出中出现架构更改，输出格式也需要固定的架构（CSV 和 Avro）。
* 如果作业重新启动，可选择在外部由用户停止或启动，或在内部进行系统维护或错误恢复。
* 如果对查询进行完全分区，会为每个输出分区创建新文件。
* 如果用户删除存储帐户的文件或容器。
* 如果使用路径前缀模式对输出进行了时间分区，当查询移动到下一个小时后，会使用新的 blob。
* 如果按自定义字段对输出进行分区，则每个分区键都会创建新的 blob（如果不存在）。
* 如果按照自定义字段对输出进行分区（其中分区键基数超过 8,000），并为每个分区键创建一个新的 blob。

## <a name="event-hubs"></a>事件中心

[Azure 事件中心](https://docs.azure.cn/zh-cn/event-hubs/)服务是具有高扩展性的发布 - 订阅事件引入器。 事件中心每秒可收集数百万个事件。 当流分析作业的输出成为另一个流式处理作业的输入时，可以将事件中心用作输出。 若要了解最大消息大小和批大小优化，请参阅[输出批大小](#output-batch-size)部分。

需要使用几个参数将事件中心的数据流配置为输出。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 | 查询中使用的易记名称，用于将查询输出定向到此事件中心。 |
| 事件中心命名空间 | 一组消息处理实体的容器。 创建新的事件中心后，还创建了事件中心命名空间。 |
| 事件中心名称 | 事件中心输出的名称。 |
| 事件中心策略名称 | 共享访问策略，可以在事件中心的“配置”选项卡上创建。每个共享访问策略具有名称、所设权限以及访问密钥。 |
| 事件中心策略密钥 | 用于对事件中心命名空间的访问权限进行身份验证的共享访问密钥。 |
| 分区键列 | 可选。 包含事件中心输出的分区键的列。 |
| 事件序列化格式 | 输出数据的序列化格式。 支持 JSON、CSV 和 Avro。 |
| 编码 | 对于 CSV 和 JSON，目前只支持 UTF-8 这种编码格式。 |
| 分隔符 | 仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 格式的数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |
| 格式 | 仅适用于 JSON 序列化。 分隔行指定通过新行分隔各个 JSON 对象，从而格式化输出。 如果选择“行分隔”，则读取 JSON 时，一次读取一个对象。 整个内容本身将不是有效的 JSON。 数组指定输出会被格式化为 JSON 对象的数组。  |
| 属性列 | 可选。 需要作为传出消息（而不是有效负载）的用户属性附加的以逗号分隔的列。 [输出的自定义元数据属性](#custom-metadata-properties-for-output)部分详细介绍了此功能。 |

<!-- Not Available ## Power BI-->
## <a name="table-storage"></a>表存储

[Azure 表存储](../storage/common/storage-introduction.md)提供了具有高可用性且可大规模缩放的存储，因此应用程序可以自动缩放以满足用户需求。 表存储是 Azure 推出的 NoSQL 键/属性存储，适用于对架构的约束较少的结构化数据。 Azure 表存储可用于持久地存储数据，方便进行高效的检索。

下表列出了用于创建表输出的属性名称及其说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |该名称是在查询中使用的友好名称，用于将查询输出定向到此表存储。 |
| 存储帐户 |存储帐户的名称（正在向该存储帐户发送输出）。 |
| 存储帐户密钥 |与存储帐户关联的访问密钥。 |
| 表名称 |表的名称。 如果表不存在，则创建该表。 |
| 分区键 |包含分区键的输出列的名称。 分区键是某个表中分区的唯一标识符，分区键构成了实体主键的第一部分。 它是最大可为 1 KB 的字符串值。 |
| 行键 |包含行键的输出列的名称。 行键是分区中实体的唯一标识符。 行键构成了实体主键的第二部分。 行键是一个最大为 1 KB 的字符串值。 |
| 批大小 |批处理操作的记录数。 默认值 (100) 对大部分作业来说都已足够。 有关修改设置的详细信息，请参阅[表批处理操作规范](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.table.tablebatchoperation)。 |

## <a name="service-bus-queues"></a>服务总线队列

[服务总线队列](../service-bus-messaging/service-bus-queues-topics-subscriptions.md)为一个或多个竞争使用方提供 FIFO 消息传递方式。 通常，接收方按照消息添加到队列中的临时顺序来接收并处理消息。 每条消息仅由一个消息使用者接收和处理。

在[兼容性级别 1.2](stream-analytics-compatibility-level.md) 中，Azure 流分析使用[高级消息队列协议 (AMQP)](../service-bus-messaging/service-bus-amqp-overview.md) 消息传递协议将内容写入服务总线队列和主题。 通过 AMQP 可使用开放标准协议构建跨平台的混合应用程序。

下表列出了用于创建队列输出的属性名称及其说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |该名称是在查询中使用的易记名称，用于将查询输出定向到此服务总线队列。 |
| 服务总线命名空间 |一组消息处理实体的容器。 |
| 队列名称 |服务总线队列的名称。 |
| 队列策略名称 |创建队列时，还可以在“队列配置”选项卡上创建共享的访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 |
| 队列策略密钥 |用于对服务总线命名空间的访问权限进行身份验证的共享访问密钥。 |
| 事件序列化格式 |输出数据的序列化格式。 支持 JSON、CSV 和 Avro。 |
| 编码 |对于 CSV 和 JSON，目前只支持 UTF-8 这种编码格式。 |
| 分隔符 |仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 格式的数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |
| 格式 |仅适用于 JSON 类型。 分隔行指定通过新行分隔各个 JSON 对象，从而格式化输出。 如果选择“行分隔”，则读取 JSON 时，一次读取一个对象。 整个内容本身将不是有效的 JSON。 数组指定输出会被格式化为 JSON 对象的数组。 |
| 属性列 | 可选。 需要作为传出消息（而不是有效负载）的用户属性附加的以逗号分隔的列。 [输出的自定义元数据属性](#custom-metadata-properties-for-output)部分详细介绍了此功能。 |
| 系统属性列 | 可选。 需要附加到传出消息而不是附加到有效负载的系统属性和相应列名的键值对。 [服务总线队列和主题输出的系统属性](#system-properties-for-service-bus-queue-and-topic-outputs)部分详细介绍了此功能。  |

分区数[基于服务总线 SKU 和大小](../service-bus-messaging/service-bus-partitioning.md)。 分区键是每个分区的唯一整数值。

## <a name="service-bus-topics"></a>服务总线主题
服务总线队列提供从发送方到接收方的一对一通信方法。 [服务总线主题](https://msdn.microsoft.com/library/azure/hh367516.aspx)提供一对多形式的通信。

下表列出了用于创建服务总线主题输出的属性名称及其说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |该名称是在查询中使用的易记名称，用于将查询输出定向到此服务总线主题。 |
| 服务总线命名空间 |包含一组消息实体的容器。 创建新的事件中心后，还创建了 Service Bus 命名空间。 |
| 主题名称 |主题是消息传递实体，类似于事件中心和队列。 设计用于从设备和服务收集事件流。 在创建主题时，还会为其提供特定的名称。 发送到主题的消息在创建订阅后才会提供给用户，因此请确保主题下存在一个或多个订阅 |
| 主题策略名称 |创建服务总线主题时，还可以在主题的“配置”选项卡上创建共享的访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 |
| 主题策略密钥 |用于对服务总线命名空间的访问权限进行身份验证的共享访问密钥。 |
| 事件序列化格式 |输出数据的序列化格式。 支持 JSON、CSV 和 Avro。 |
| 编码 |如果使用 CSV 或 JSON 格式，则必须指定一种编码格式。 目前只支持 UTF-8 这种编码格式。 |
| 分隔符 |仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 格式的数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |
| 属性列 | 可选。 需要作为传出消息（而不是有效负载）的用户属性附加的以逗号分隔的列。 [输出的自定义元数据属性](#custom-metadata-properties-for-output)部分详细介绍了此功能。 |
| 系统属性列 | 可选。 需要附加到传出消息而不是附加到有效负载的系统属性和相应列名的键值对。 [服务总线队列和主题输出的系统属性](#system-properties-for-service-bus-queue-and-topic-outputs)部分详细介绍了此功能。 |

分区数[基于服务总线 SKU 和大小](../service-bus-messaging/service-bus-partitioning.md)。 分区键是每个分区的唯一整数值。

<!-- Not Available on ## Azure Cosmos DB -->


<!-- Not Available on ## Azure Functions-->
## <a name="custom-metadata-properties-for-output"></a>输出的自定义元数据属性 

可将查询列作为用户属性附加到传出的消息。 这些列不会进入有效负载。 这些属性以字典形式在输出消息中提供。 键是列名，值是属性字典中的列值。  支持除“记录”和“数组”以外的其他所有流分析数据类型。  

支持的输出： 
* 服务总线队列 
* 服务总线主题 
* 事件中心 

以下示例将 `DeviceId` 和 `DeviceStatus` 这两个字段添加到了元数据。 
* 查询：`select *, DeviceId, DeviceStatus from iotHubInput`
* 输出配置：`DeviceId,DeviceStatus`

![属性列](./media/stream-analytics-define-outputs/10-stream-analytics-property-columns.png)

以下屏幕截图显示了在事件中心通过[服务总线资源管理器](https://github.com/paolosalvatori/ServiceBusExplorer)检查的输出消息属性。

![事件自定义属性](./media/stream-analytics-define-outputs/09-stream-analytics-custom-properties.png)

## <a name="system-properties-for-service-bus-queue-and-topic-outputs"></a>服务总线队列和主题输出的系统属性 
可以将查询列作为[系统属性](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet#properties)附加到传出的服务总线队列或主题消息。 这些列不会进入有效负载，而是将查询列值填充到相应的 BrokeredMessage [系统属性](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet#properties)中。
支持这些系统属性 - `MessageId, ContentType, Label, PartitionKey, ReplyTo, SessionId, CorrelationId, To, ForcePersistence, TimeToLive, ScheduledEnqueueTimeUtc`。
这些列的字符串值将分析成相应的系统属性值类型，任何分析失败将被视为数据错误。
此字段以 JSON 对象格式提供。 有关此格式的详细信息如下：
* 用大括号 {} 括住。
* 以键值对的形式编写。
* 键和值必须是字符串。
* 键是系统属性名称，值是查询列名。
* 键和值以冒号分隔。
* 每个键值对以逗号分隔。

下面演示了此属性的用法 -

* 查询：`select *, column1, column2 INTO queueOutput FROM iotHubInput`
* 系统属性列：`{ "MessageId": "column1", "PartitionKey": "column2"}`

这会在服务总线队列消息的 `MessageId` 中设置 `column1` 的值，并在 PartitionKey 中设置 `column2` 的值。

## <a name="partitioning"></a>分区

下表汇总了分区支持，以及每个输出类型的输出写入器数目：

| 输出类型 | 分区支持 | 分区键  | 输出写入器数目 |
| --- | --- | --- | --- |
| Azure Data Lake Store | 是 | 在路径前缀模式中使用 {date} 和 {time} 标记。 选择日期格式，例如 YYYY/MM/DD、DD/MM/YYYY 或 MM-DD-YYYY。 HH 用于时间格式。 | 按照[完全可并行化的查询](stream-analytics-scale-jobs.md)的输入分区。 |
| Azure SQL 数据库 | 是，但需要启用。 | 基于查询中的 PARTITION BY 子句。 | 如果已启用“继承分区”选项，请遵循[完全可并行化的查询](stream-analytics-scale-jobs.md)的输入分区。 若要详细了解在将数据载入 Azure SQL 数据库时如何提高写入吞吐量性能，请参阅[从 Azure 流分析输出到 Azure SQL 数据库](stream-analytics-sql-output-perf.md)。 |
| Azure Blob 存储 | 是 | 在路径模式中使用事件字段中的 {date} 和 {time} 标记。 选择日期格式，例如 YYYY/MM/DD、DD/MM/YYYY 或 MM-DD-YYYY。 HH 用于时间格式。 可以通过单个自定义事件属性 {fieldname} 或 {datetime:\<specifier>} 对 Blob 输出进行分区。 | 按照[完全可并行化的查询](stream-analytics-scale-jobs.md)的输入分区。 |
| Azure 事件中心 | 是 | 是 | 按分区对齐方式变化。<br /> 如果事件中心输出的分区键与上游（上一个）查询步骤不相符，写入器的数量与事件中心输出中的分区数量相同。 每个写入器使用 [EventHubSender 类](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet)将事件发送到特定分区。 <br /> 如果事件中心输出的分区键与上游（上一个）查询步骤不相符，写入器的数量与该上一步骤中的分区数量相同。 每个写入器使用 **EventHubClient** 中的 [SendBatchAsync 类](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet)将事件发送到所有输出分区。 |
| Power BI | 否 | 无 | 不适用。 |
| Azure 表存储 | 是 | 任何输出列。  | 按照[完全并行化的查询](stream-analytics-scale-jobs.md)的输入分区。 |
| Azure 服务总线主题 | 是 | 自动选择。 分区数基于[服务总线 SKU 和大小](../service-bus-messaging/service-bus-partitioning.md)。 分区键是每个分区的唯一整数值。| 与输出主题中的分区数量相同。  |
| Azure 服务总线队列 | 是 | 自动选择。 分区数基于[服务总线 SKU 和大小](../service-bus-messaging/service-bus-partitioning.md)。 分区键是每个分区的唯一整数值。| 与输出队列中的分区数量相同。 |
| Azure Cosmos DB | 是 | 基于查询中的 PARTITION BY 子句。 | 按照[完全并行化的查询](stream-analytics-scale-jobs.md)的输入分区。 |
| Azure Functions | 是 | 基于查询中的 PARTITION BY 子句。 | 按照[完全并行化的查询](stream-analytics-scale-jobs.md)的输入分区。 |

还可以在查询中使用 `INTO <partition count>`子句（请参阅 [INTO](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)）来控制输出写入器的数量，这可能有助于实现所需的作业拓扑。 如果输出适配器未分区，则一个输入分区中缺少数据将导致延迟最多可达延迟到达的时间量。 在这种情况下，输出将合并到单个写入器，这可能会导致管道中出现瓶颈。

## <a name="output-batch-size"></a>输出批大小
Azure 流分析使用大小可变的批来处理事件和写入到输出。 通常流分析引擎不会一次写入一条消息，而是使用批来提高效率。 当传入和传出事件的速率较高时，流分析将使用更大的批。 输出速率低时，使用较小的批来保证低延迟。

下表阐述了输出批处理的一些注意事项：

| 输出类型 |    最大消息大小 | 批大小优化 |
| :--- | :--- | :--- |
| Azure Data Lake Store | 请参阅 [Data Lake Storage 限制](../azure-resource-manager/management/azure-subscription-service-limits.md)。 | 每个写入操作最高 4 MB。 |
| Azure SQL 数据库 | 可使用最大批计数进行配置。 默认情况下，单个批量插入操作最多可插入 10,000 行，最少可插入 100 行。<br />请参阅 [Azure SQL 限制](../sql-database/sql-database-resource-limits.md)。 |  每个批最初是按照最大批计数批量插入的。 根据 SQL 的可重试错误对半拆分批（直到达到最小批计数）。 |
| Azure Blob 存储 | 参阅 [Azure 存储限制](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits)。 | 最大 Blob 块大小为 4 MB。<br />最大 Blob 块计数为 50,000。 |
| Azure 事件中心    | 每条消息 256 KB 或 1 MB。 <br />请参阅[事件中心限制](../event-hubs/event-hubs-quotas.md)。 |    如果输入/输出分区未对齐，则每个事件将单独打包成不超过最大消息大小的 `EventData`，并在批中发送。 如果使用[自定义元数据属性](#custom-metadata-properties-for-output)，也会发生这种情况。 <br /><br />  如果输入/输出分区已对齐，则多个事件将打包成不超过最大消息大小的单个 `EventData` 实例，然后发送。    |
| Power BI | 请参阅 [Power BI REST API 限制](https://msdn.microsoft.com/library/dn950053.aspx)。 |
| Azure 表存储 | 参阅 [Azure 存储限制](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits)。 | 默认为单个事务 100 个实体。 可根据需要将其配置为更小的值。 |
| Azure 服务总线队列    | 在标准层中为每条消息 256 KB，在高级层中为 1MB。<br /> 请参阅[服务总线限制](../service-bus-messaging/service-bus-quotas.md)。 | 对每条消息使用单个事件。 |
| Azure 服务总线主题 | 在标准层中为每条消息 256 KB，在高级层中为 1MB。<br /> 请参阅[服务总线限制](../service-bus-messaging/service-bus-quotas.md)。 | 对每条消息使用单个事件。 |
| Azure Cosmos DB    | 请参阅 [Azure Cosmos DB 限制](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-cosmos-db-limits)。 | 批大小和写入频率根据 Azure Cosmos DB 响应动态调整。 <br /> 流分析不会施加预先确定的限制。 |
| Azure Functions    | | 默认批大小为 262,144 字节 (256 KB)。 <br /> 每批的默认事件计数为 100。 <br /> 批大小是可配置的，可在流分析输出选项中增大或减小。

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> 
> [快速入门：使用 Azure 门户创建流分析作业](stream-analytics-quick-create-portal.md)

<!--Link references-->
<!-- URL is not Correct on [stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md -->

[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

<!--Update_Description: update link, wroding update -->

