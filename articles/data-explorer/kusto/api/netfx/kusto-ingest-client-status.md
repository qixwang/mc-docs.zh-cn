---
title: Kusto.Ingest 状态报告 - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的 Kusto.Ingest 引入状态报告。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/30/2019
ms.date: 07/01/2020
ms.openlocfilehash: 63053ee767bbd376a744ca052935aef381e7729b
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226349"
---
# <a name="kustoingest-ingestion-status-reporting"></a>Kusto.Ingest 引入状态报告

本文介绍了如何使用 [IKustoQueuedIngestClient](kusto-ingest-client-reference.md#interface-ikustoqueuedingestclient) 功能来跟踪引入请求的状态。

## <a name="description-classes"></a>说明类

这些说明类包含有关要引入的源数据的重要详细信息，应在引入操作中使用。 

* SourceDescription
* DataReaderDescription
* StreamDescription
* FileDescription
* BlobDescription

这些类全部派生自抽象类 `SourceDescription`，用于实例化每个数据源的唯一标识符。 然后，系统会使用每个标识符进行状态跟踪并将其显示在与相关操作相关的所有报告、跟踪和异常中。

### <a name="class-sourcedescription"></a>Class SourceDescription

大型数据集将拆分为 1GB 的区块，每个部分将单独引入。 然后，系统会将同一 SourceId 应用于源自同一数据集的所有引入操作。   

```csharp
public abstract class SourceDescription
{
    public Guid? SourceId { get; set; }
}
```

### <a name="class-datareaderdescription"></a>Class DataReaderDescription

```csharp
public class DataReaderDescription : SourceDescription
{
    public  IDataReader DataReader { get; set; }
}
```

### <a name="class-streamdescription"></a>Class StreamDescription

```csharp
public class StreamDescription : SourceDescription
{
    public Stream Stream { get; set; }
}
```

### <a name="class-filedescription"></a>Class FileDescription

```csharp
public class FileDescription : SourceDescription
{
    public string FilePath { get; set; }
}
```

### <a name="class-blobdescription"></a>Class BlobDescription

```csharp
public class BlobDescription : SourceDescription
{
    public string BlobUri { get; set; }
    // Setting the Blob size here is important, as this saves the client the trouble of probing the blob for size
    public long? BlobSize { get; set; }
}
```

## <a name="ingestion-result-representation"></a>引入结果表示形式

### <a name="interface-ikustoingestionresult"></a>Interface IKustoIngestionResult

此接口捕获单个排队引入操作的结果，可通过 `SourceId` 来检索。

```csharp
public interface IKustoIngestionResult
{
    // Retrieves the detailed ingestion status of the ingestion source with the given sourceId.
    IngestionStatus GetIngestionStatusBySourceId(Guid sourceId);

    // Retrieves the detailed ingestion status of all data ingestion operations into Kusto associated with this IKustoIngestionResult instance.
    IEnumerable<IngestionStatus> GetIngestionStatusCollection();
}
```

### <a name="class-ingestionstatus"></a>Class IngestionStatus

IngestionStatus 包含单个引入操作的完整状态。

```csharp
public class IngestionStatus
{
    // The ingestion status returns from the service. Status remains 'Pending' during the ingestion process and
    // is updated by the service once the ingestion completes. When <see cref="IngestionReportMethod"/> is set to 'Queue' the ingestion status
    // will always be 'Queued' and the caller needs to query the report queues for ingestion status, as configured. To query statuses that were
    // reported to queue, see: <see href="https://docs.azure.cn/kusto/api/netfx/kusto-ingest-client-status#ingestion-status-in-azure-queue"/>.
    // When <see cref="IngestionReportMethod"/> is set to 'Table', call <see cref="IKustoIngestionResult.GetIngestionStatusBySourceId"/> or
    // <see cref="IKustoIngestionResult.GetIngestionStatusCollection"/> to retrieve the most recent ingestion status.
    public Status Status { get; set; }
    // A unique identifier representing the ingested source. Can be supplied during the ingestion execution.
    public Guid IngestionSourceId { get; set; }
    // The URI of the blob, potentially including the secret needed to access the blob.
    // This can be a filesystem URI (on-premises deployments only), or an Azure Blob Storage URI (including a SAS key or a semicolon followed by the account key)
    public string IngestionSourcePath { get; set; }
    // The name of the database holding the target table.
    public string Database { get; set; }
    // The name of the target table into which the data will be ingested.
    public string Table { get; set; }
    // The last updated time of the ingestion status.
    public DateTime UpdatedOn { get; set; }
    // The ingestion's operation Id.
    public Guid OperationId { get; set; }
    // The ingestion operation activity Id.
    public Guid ActivityId { get; set; }
    // In case of a failure - indicates the failure's error code.
    public IngestionErrorCode ErrorCode { get; set; }
    // In case of a failure - indicates the failure's status.
    public FailureStatus FailureStatus { get; set; }
    // In case of a failure - indicates the failure's details.
    public string Details { get; set; }
    // In case of a failure - indicates whether or not the failures originate from an Update Policy.
    public bool OriginatesFromUpdatePolicy { get; set; }
}
```

### <a name="status-enumeration"></a>状态枚举

|Value              |含义                                                                                     |临时/永久
|-------------------|-----------------------------------------------------------------------------------------------------|---------|
|挂起            |此值在引入过程中可能会发生变化，具体取决于引入操作的结果 |临时|
|已成功          |数据已成功引入                                                              |永久性| 
|已失败             |引入失败                                                                                     |永久性|
|已排队             |数据已排队等待引入                                                               |永久性|
|已跳过            |未提供任何数据，已跳过引入操作                                            |永久性|
|PartiallySucceeded |部分数据引入成功，部分数据引入失败                                        |永久性|

## <a name="tracking-ingestion-status-kustoqueuedingestclient"></a>跟踪引入状态 (KustoQueuedIngestClient)

[IKustoQueuedIngestClient](kusto-ingest-client-reference.md#interface-ikustoqueuedingestclient) 是一种“即发即弃”客户端。 客户端上的引入操作将消息发布到 Azure 队列便结束了。 在发布后，客户端作业完成。 为了方便客户端用户，KustoQueuedIngestClient 提供了一种用于跟踪各个引入状态的机制。 此机制不适合在高吞吐量引入管道上大量使用。 此机制适用于在速率相对较低且跟踪要求严格的情况下进行精确引入。

> [!WARNING]
> 应避免为大容量数据流的每个引入请求启用主动通知，因为这会给基础 xStore 资源带来极大的负载，因此可能会导致引入延迟增加，甚至可能会导致群集根本不响应。

以下属性（在 [KustoQueuedIngestionProperties](kusto-ingest-client-reference.md#class-kustoqueuedingestionproperties) 上设置的）控制引入成功通知或失败通知的级别和传输。

### <a name="ingestionreportlevel-enumeration"></a>IngestionReportLevel 枚举

```csharp
public enum IngestionReportLevel
{
    FailuresOnly = 0,
    None,
    FailuresAndSuccesses
}
```

### <a name="ingestionreportmethod-enumeration"></a>IngestionReportMethod 枚举

```csharp
public enum IngestionReportMethod
{
    Queue = 0,
    Table,
    QueueAndTable
}
```

若要跟踪引入状态，请为执行引入操作时使用的 [IKustoQueuedIngestClient](kusto-ingest-client-reference.md#interface-ikustoqueuedingestclient) 提供以下内容：
1.  将 `IngestionReportLevel` 属性设置为所需的报告级别。 `FailuresOnly`（默认值）或 `FailuresAndSuccesses`。
如果设置为 `None`，则在引入结束时不会报告任何内容。
1.  指定 `IngestionReportMethod` - `Queue`、`Table` 或 `both`。

可在 [Kusto.Ingest 示例](kusto-ingest-client-examples.md)页上找到用法示例。

### <a name="ingestion-status-in-the-azure-table"></a>Azure 表中的引入状态

从每个引入操作返回的 `IKustoIngestionResult` 接口包含可用于查询引入状态的函数。
请特别注意返回的 `IngestionStatus` 对象的 `Status` 属性：
* `Pending` 表示源已排队等待引入，但尚未更新。 请再次使用该函数查询源的状态
* `Succeeded` 表示已成功引入源
* `Failed` 表示未能引入源

> [!NOTE]
> 如果得到 `Queued` 状态，则表示 `IngestionReportMethod` 保留了其默认值“Queue”。 这是永久性状态，重新调用 `GetIngestionStatusBySourceId` 或 `GetIngestionStatusCollection` 函数将始终产生同一状态：“Queued”。
> 若要检查 Azure 表中某个引入的状态，请在引入之前验证 [KustoQueuedIngestionProperties](kusto-ingest-client-reference.md#class-kustoqueuedingestionproperties) 的 `IngestionReportMethod` 属性是否已设置为 `Table`。 如果还想将引入状态报告给队列，请将状态设置为 `QueueAndTable`。

### <a name="ingestion-status-in-azure-queue"></a>Azure 队列中的引入状态

`IKustoIngestionResult` 方法仅适用于检查 Azure 表中的状态。 若要查询报告给 Azure 队列的状态，请使用下面的 [IKustoQueuedIngestClient](kusto-ingest-client-reference.md#interface-ikustoqueuedingestclient) 方法。

|方法                                  |目的     |
|----------------------------------------|------------|
|PeekTopIngestionFailures                |一种异步方法，该方法返回的信息涉及由于对所请求的消息存在限制而尚未丢弃的最早的引入失败 |
|GetAndDiscardTopIngestionFailures       |一种异步方法，该方法返回并丢弃由于对所请求的消息存在限制而尚未丢弃的最早的引入失败 |
|GetAndDiscardTopIngestionSuccesses      |一种异步方法，该方法返回并丢弃由于对所请求的消息存在限制而尚未丢弃的最早的引入成功。 只有当 `IngestionReportLevel` 设置为 `FailuresAndSuccesses` 时，此方法才适用。 |

### <a name="ingestion-failures-retrieved-from-the-azure-queue"></a>从 Azure 队列检索的引入失败

引入失败由 `IngestionFailure` 对象表示，该对象包含有关失败的有用信息。

|属性                      |含义     |
|------------------------------|------------|
|数据库和表              |所需的数据库名称和表名称 |
|IngestionSourcePath           |引入的 blob 的路径。 如果文件已引入，此项会包含原始文件名。 如果 DataReader 已引入，则此项将是随机的 |
|FailureStatus                 |`Permanent`（将不执行重试）、`Transient`（将执行重试）或 `Exhausted`（多次重试也都失败） |
|OperationId 和 RootActivityId  |引入的操作 ID 和 RootActivity ID（用于进一步故障排除） |
|FailedOn                      |失败时间 (UTC)。 将晚于调用引入方法的时间，因为在运行引入操作之前会聚合数据 |
|详细信息                       |有关失败的其他详细信息（如果有） |
|ErrorCode                     |`IngestionErrorCode` 枚举，表示引入错误代码（如果失败） |
