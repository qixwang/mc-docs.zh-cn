---
title: Kusto.Ingest 代码示例 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 Kusto.Ingest 代码示例。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: ohbitton
ms.service: data-explorer
ms.topic: reference
origin.date: 05/19/2019
ms.date: 07/01/2020
ms.openlocfilehash: e8b2fe9b5e440062a06ea8e041ecfe8e89cc0558
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226326"
---
# <a name="kustoingest-ingestion-code-examples"></a>Kusto.Ingest 引入代码示例

此简短代码片段集合演示了将数据引入到 Kusto 表的各种技术。

> [!NOTE]
> 这些示例给人的印象就是在引入之后会立即销毁引入客户端。 请不要片面理解。
> 引入客户端是可以重入并且是线程安全的，不应大量创建。 建议将引入客户端实例的基数设置为每个托管进程一个，每个目标 Kusto 群集一个。

## <a name="async-ingestion-from-a-single-azure-blob"></a>从单个 Azure blob 异步引入

使用带有可选 RetryPolicy 的 KustoQueuedIngestClient 从单个 Azure blob 异步引入。

```csharp
//Create Kusto connection string with App Authentication
var kustoConnectionStringBuilderDM =
    new KustoConnectionStringBuilder(@"https://ingest-{clusterNameAndRegion}.kusto.chinacloudapi.cn").WithAadApplicationKeyAuthentication(
        applicationClientId: "{Application Client ID}",
        applicationKey: "{Application Key (secret)}",
        authority: "{AAD TenantID or name}");

// Create an ingest client
// Note, that creating a separate instance per ingestion operation is an anti-pattern.
// IngestClient classes are thread-safe and intended for reuse
IKustoIngestClient client = KustoIngestFactory.CreateQueuedIngestClient(kustoConnectionStringBuilderDM);

// Ingest from blobs according to the required properties
var kustoIngestionProperties = new KustoIngestionProperties(databaseName: "myDB", tableName: "myTable");

var sourceOptions = new StorageSourceOptions() { DeleteSourceOnSuccess = true };

//// Create your custom implementation of IRetryPolicy, which will affect how the ingest client handles retrying on transient failures
IRetryPolicy retryPolicy = new NoRetry();
//// This line sets the retry policy on the ingest client that will be enforced on every ingest call from here on
((IKustoQueuedIngestClient)client).QueueRetryPolicy = retryPolicy;

await client.IngestFromStorageAsync(uri: @"BLOB-URI-WITH-SAS-KEY", ingestionProperties: kustoIngestionProperties, sourceOptions);

client.Dispose();
```

## <a name="ingest-from-local-file"></a>从本地文件引入 

使用 KustoDirectIngestClient 从本地文件引入。


> [!NOTE]
> 对于有限数量的低频率引入，建议采用此方法。

```csharp
// Create Kusto connection string with App Authentication
var kustoConnectionStringBuilderEngine =
    new KustoConnectionStringBuilder(@"https://{clusterNameAndRegion}.kusto.chinacloudapi.cn").WithAadApplicationKeyAuthentication(
        applicationClientId: "{Application Client ID}",
        applicationKey: "{Application Key (secret)}",
        authority: "{AAD TenantID or name}");

// Create a disposable client that will execute the ingestion
using (IKustoIngestClient client = KustoIngestFactory.CreateDirectIngestClient(kustoConnectionStringBuilderEngine))
{
    //Ingest from blobs according to the required properties
    var kustoIngestionProperties = new KustoIngestionProperties(databaseName: "myDB", tableName: "myTable");

    client.IngestFromStorageAsync(@"< Path to local file >", ingestionProperties: kustoIngestionProperties).GetAwaiter().GetResult();
}
```

## <a name="ingest-from-local-files-and-validate-ingestion"></a>从本地文件引入并验证引入

使用 KustoQueuedIngestClient 从本地文件引入，然后验证引入。

```csharp
// Create Kusto connection string with App Authentication
var kustoConnectionStringBuilderDM =
    new KustoConnectionStringBuilder(@"https://ingest-{clusterNameAndRegion}.kusto.chinacloudapi.cn").WithAadApplicationKeyAuthentication(
        applicationClientId: "{Application Client ID}",
        applicationKey: "{Application Key (secret)}",
        authority: "{AAD TenantID or name}");

// Create a disposable client that will execute the ingestion
IKustoQueuedIngestClient client = KustoIngestFactory.CreateQueuedIngestClient(kustoConnectionStringBuilderDM);

// Ingest from files according to the required properties
var kustoIngestionProperties = new KustoIngestionProperties(databaseName: "myDB", tableName: "myTable");

client.IngestFromStorageAsync(@"ValidTestFile.csv", kustoIngestionProperties);
client.IngestFromStorageAsync(@"InvalidTestFile.csv", kustoIngestionProperties);

// Waiting for the aggregation
Thread.Sleep(TimeSpan.FromMinutes(8));

// Retrieve and validate failures
var ingestionFailures = client.PeekTopIngestionFailuresAsync().GetAwaiter().GetResult();
Ensure.IsTrue((ingestionFailures.Count() > 0), "Failures expected");
// Retrieve, delete and validate failures
ingestionFailures = client.GetAndDiscardTopIngestionFailuresAsync().GetAwaiter().GetResult();
Ensure.IsTrue((ingestionFailures.Count() > 0), "Failures expected");

// Dispose of the client
client.Dispose();
```

### <a name="ingest-from-local-files-and-report-status-to-a-queue"></a>从本地文件引入并向队列报告状态

使用 KustoQueuedIngestClient 从本地文件引入，然后向队列报告状态。

```csharp
// Create Kusto connection string with App Authentication
var kustoConnectionStringBuilderDM =
    new KustoConnectionStringBuilder(@"https://ingest-{clusterNameAndRegion}.kusto.chinacloudapi.cn").WithAadApplicationKeyAuthentication(
        applicationClientId: "{Application Client ID}",
        applicationKey: "{Application Key (secret)}",
        authority: "{AAD TenantID or name}");

// Create a disposable client that will execute the ingestion
IKustoQueuedIngestClient client = KustoIngestFactory.CreateQueuedIngestClient(kustoConnectionStringBuilderDM);

// Ingest from a file according to the required properties
var kustoIngestionProperties = new KustoQueuedIngestionProperties(databaseName: "myDB", tableName: "myTable")
{
    // Setting the report level to FailuresAndSuccesses will cause both successful and failed ingestions to be reported
    // (Rather than the default "FailuresOnly" level - which is demonstrated in the
    // 'Ingest From Local File(s) using KustoQueuedIngestClient and Ingestion Validation' section)
    ReportLevel = IngestionReportLevel.FailuresAndSuccesses,
    // Choose the report method of choice. 'Queue' is the default method.
    // For the sake of the example, we will choose it anyway. 
    ReportMethod = IngestionReportMethod.Queue
};

client.IngestFromStorageAsync("ValidTestFile.csv", kustoIngestionProperties);
client.IngestFromStorageAsync("InvalidTestFile.csv", kustoIngestionProperties);

// Waiting for the aggregation
Thread.Sleep(TimeSpan.FromMinutes(8));

// Retrieve and validate failures
var ingestionFailures = client.PeekTopIngestionFailures().GetAwaiter().GetResult();
Ensure.IsTrue((ingestionFailures.Count() > 0), "The failed ingestion should have been reported to the failed ingestions queue");
// Retrieve, delete and validate failures
ingestionFailures = client.GetAndDiscardTopIngestionFailures().GetAwaiter().GetResult();
Ensure.IsTrue((ingestionFailures.Count() > 0), "The failed ingestion should have been reported to the failed ingestions queue");

// Verify the success has also been reported to the queue
var ingestionSuccesses = client.GetAndDiscardTopIngestionSuccesses().GetAwaiter().GetResult();
Ensure.ConditionIsMet((ingestionSuccesses.Count() > 0),
    "The successful ingestion should have been reported to the successful ingestions queue");

// Dispose of the client
client.Dispose();
```

### <a name="ingest-from-local-files-and-report-status-to-a-table"></a>从本地文件引入并向表报告状态

使用 KustoQueuedIngestClient 从本地文件引入，然后向表报告状态。

```csharp
// Create Kusto connection string with App Authentication
var kustoConnectionStringBuilderDM =
    new KustoConnectionStringBuilder(@"https://ingest-{clusterNameAndRegion}.kusto.chinacloudapi.cn").WithAadApplicationKeyAuthentication(
        applicationClientId: "{Application Client ID}",
        applicationKey: "{Application Key (secret)}",
        authority: "{AAD TenantID or name}");

// Create a disposable client that will execute the ingestion
IKustoQueuedIngestClient client = KustoIngestFactory.CreateQueuedIngestClient(kustoConnectionStringBuilderDM);

// Ingest from a file according to the required properties
var kustoIngestionProperties = new KustoQueuedIngestionProperties(databaseName: "myDB", tableName: "myDB")
{
    // Setting the report level to FailuresAndSuccesses will cause both successful and failed ingestions to be reported
    // (Rather than the default "FailuresOnly" level)
    ReportLevel = IngestionReportLevel.FailuresAndSuccesses,
    // Choose the report method of choice
    ReportMethod = IngestionReportMethod.Table
};

var filePath = @"< Path to file >";
var fileIdentifier = Guid.NewGuid();
var fileDescription = new FileDescription() { FilePath = filePath, SourceId = fileIdentifier };
var sourceOptions = new StorageSourceOptions() { SourceId = fileDescription.SourceId.Value };

// Execute the ingest operation and save the result.
var clientResult = await client.IngestFromStorageAsync(fileDescription.FilePath,
    ingestionProperties: kustoIngestionProperties, sourceOptions);

// Use the fileIdentifier you supplied to get the status of your ingestion 
var ingestionStatus = clientResult.GetIngestionStatusBySourceId(fileIdentifier);
while (ingestionStatus.Status == Status.Pending)
{
    // Wait a minute...
    Thread.Sleep(TimeSpan.FromMinutes(1));
    // Try again
    ingestionStatus = clientResult.GetIngestionStatusBySourceId(fileIdentifier);
}

// Verify the results of the ingestion
Ensure.ConditionIsMet(ingestionStatus.Status == Status.Succeeded,
    "The file should have been ingested successfully");

// Dispose of the client
client.Dispose();
```

## <a name="next-steps"></a>后续步骤

* [Kusto.Ingest 客户端参考](kusto-ingest-client-reference.md)
* [Kusto.Ingest 操作状态](kusto-ingest-client-errors.md)
* [Kusto.Ingest 异常](kusto-ingest-client-errors.md)
* [Kusto 连接字符串](../connection-strings/kusto.md)
* [Kusto 授权模型](../../management/security-roles.md)
