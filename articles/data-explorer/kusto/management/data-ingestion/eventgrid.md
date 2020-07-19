---
title: 使用事件网格订阅从存储引入 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了如何在 Azure 数据资源管理器中使用事件网格订阅从存储引入。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 04/01/2020
ms.date: 07/01/2020
ms.openlocfilehash: e3ce34bc28b8d2e12cf40fe33f1eefb140474fd0
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226224"
---
# <a name="ingest-from-storage-using-event-grid-subscription"></a>使用事件网格订阅从存储引入

有了 [Azure 事件网格](https://docs.azure.cn/event-grid/overview)订阅，Azure 数据资源管理器就可以从 Azure 存储（Blob 存储和 ADLSv2）连续引入。该订阅适用于 Blob 创建的通知，可以通过事件中心将这些通知流式传输到 Kusto。

## <a name="data-format"></a>数据格式

* Blob 可以采用任何[受支持的格式](../../../ingestion-supported-formats.md)。
* 可以在任何[受支持的压缩](../../../ingestion-supported-formats.md#supported-data-compression-formats)中压缩 blob。

> [!NOTE]
> 理想情况下，原始未压缩的数据大小应当是 blob 元数据的一部分。
> 如果未指定未压缩的大小，Azure 数据资源管理器会根据文件大小估算该大小。 请通过将 blob 元数据中的 `rawSizeBytes` [属性](#ingestion-properties)设置为**未压缩**数据大小（字节）来提供原始数据大小。
> 请注意，每个文件的未压缩引入大小限制为 4GB。

## <a name="ingestion-properties"></a>引入属性

可以通过 blob 元数据指定 blob 引入的[引入属性](../../../ingestion-properties.md)。
可以设置以下属性：

|属性 | 说明|
|---|---|
| rawSizeBytes | 原始（未压缩的）数据的大小。 对于 Avro/ORC/Parquet，此值是应用特定于格式的压缩之前的大小。|
| kustoTable |  现有目标表的名称。 替代“`Data Connection`”边栏选项卡上设置的“`Table`”。 |
| kustoDataFormat |  数据格式。 替代“`Data Connection`”边栏选项卡上设置的“`Data format`”。 |
| kustoIngestionMappingReference |  要使用的现有引入映射的名称。 替代“`Data Connection`”边栏选项卡上设置的“`Column mapping`”。|
| kustoIgnoreFirstRecord | 如果设置为 `true`，则 Azure 数据资源管理器将忽略 blob 的第一行。 在表格格式数据（CSV、TSV 或类似格式）中使用将忽略标题。 |
| kustoExtentTags | 字符串，表示将附加到生成的盘区的[标记](../extents-overview.md#extent-tagging)。 |
| kustoCreationTime |  替代 blob 的 [$IngestionTime](../../query/ingestiontimefunction.md?pivots=azuredataexplorer)，格式为 ISO 8601 字符串。 用于回填。 |

## <a name="events-routing"></a>事件路由

设置到 Azure 数据资源管理器群集的 blob 存储连接时，请指定目标表属性（表名、数据格式和映射）。 此设置是用于你的数据的默认路由，也称为 `static routing`。
还可以使用 blob 元数据指定每个 blob 的目标表属性。 数据将按照[引入属性](#ingestion-properties)指定的方式动态进行路由。

下面是在上传前为 blob 元数据设置引入属性的示例。 Blob 路由到不同的表。

有关如何生成数据的更多详细信息，请参阅[示例代码](#generating-data)。

 ```csharp
// Blob is dynamically routed to table `Events`, ingested using `EventsMapping` data mapping
blob = container.GetBlockBlobReference(blobName2);
blob.Metadata.Add("rawSizeBytes", "4096‬"); // the uncompressed size is 4096 bytes
blob.Metadata.Add("kustoTable", "Events");
blob.Metadata.Add("kustoDataFormat", "json");
blob.Metadata.Add("kustoIngestionMappingReference", "EventsMapping");
blob.UploadFromFile(jsonCompressedLocalFileName);
```

## <a name="create-event-grid-subscription"></a>创建事件网格订阅

> [!Note]
> 为了获得最佳性能，请在 Azure 数据资源管理器群集所在的区域中创建所有资源。

### <a name="prerequisites"></a>先决条件

* [创建存储帐户](https://docs.azure.cn/storage/common/storage-quickstart-create-account)。 
  可以在 `BlobStorage` 或 `StorageV2` 类型的 Azure 存储帐户上设置事件网格通知订阅。 
  还支持启用 [Data Lake Storage Gen2](https://docs.azure.cn/storage/blobs/data-lake-storage-introduction)。
* [创建事件中心](https://docs.azure.cn/event-hubs/event-hubs-create)。

### <a name="event-grid-subscription"></a>事件网格订阅

* Kusto 选择了 `Event Hub` 作为终结点类型，用于传输 blob 存储事件通知。 `Event Grid schema` 是用于通知的所选架构。 每个事件中心可以为一个连接提供服务。
* Blob 存储订阅连接处理 `Microsoft.Storage.BlobCreated` 类型的通知。 请确保在创建订阅时选择该类型。 选择的其他类型的通知会被忽略。
* 一个订阅可以针对一个或多个容器中的存储事件发出通知。 如果要跟踪特定容器中的文件，请设置通知的筛选器，如下所示：设置连接时，请特别注意以下值： 
   * “主题开头为”筛选器是 Blob 容器的文本前缀。 由于应用的模式是 *startswith*，因此可以跨越多个容器。 不允许通配符。
     它必须设置如下： *`/blobServices/default/containers/<prefix>`* 。 例如： */blobServices/default/containers/StormEvents-2020-*
   * “主题末尾为”字段是 Blob 的文本后缀。 不允许通配符。 用于筛选文件扩展名。

有关详细演练，可参阅[如何在存储帐户中创建事件网格订阅](../../../ingest-data-event-grid.md#create-an-event-grid-subscription-in-your-storage-account)指南。

### <a name="data-ingestion-connection-to-azure-data-explorer"></a>与 Azure 数据资源管理器的数据引入连接

* 通过 Azure 门户：[在 Azure 数据资源管理器中创建事件网格数据连接](../../../ingest-data-event-grid.md#create-an-event-grid-data-connection-in-azure-data-explorer)。
* 使用 Kusto 管理 .NET SDK：[添加事件网格数据连接](../../../data-connection-event-grid-csharp.md#add-an-event-grid-data-connection)
* 使用 Kusto 管理 Python SDK：[添加事件网格数据连接](../../../data-connection-event-grid-python.md#add-an-event-grid-data-connection)
* 使用 ARM 模板：[用于添加事件网格数据连接的 Azure 资源管理器模板](../../../data-connection-event-grid-resource-manager.md#azure-resource-manager-template-for-adding-an-event-grid-data-connection)

### <a name="generating-data"></a>生成数据

> [!NOTE]
> * 使用 `BlockBlob` 生成数据。 不支持 `AppendBlob`。
<!--> * Using ADLSv2 storage requires using `CreateFile` API for uploading blobs and flush at the end. 
    Kusto will get 2 notificatiopns: when blob is created and when stream is flushed. First notification arrives before the data is ready and ignored. Second notification is processed.-->

下面是一个示例，展示了如何基于本地文件创建 blob、为 blob 元数据设置引入属性以及上传该元数据：

 ```csharp
 var azureStorageAccountConnectionString=<storage_account_connection_string>;

var containerName=<container_name>;
var blobName=<blob_name>;
var localFileName=<file_to_upload>;

// Creating the container
var azureStorageAccount = CloudStorageAccount.Parse(azureStorageAccountConnectionString);
var blobClient = azureStorageAccount.CreateCloudBlobClient();
var container = blobClient.GetContainerReference(containerName);
container.CreateIfNotExists();

// Set ingestion properties in blob's metadata & uploading the blob
var blob = container.GetBlockBlobReference(blobName);
blob.Metadata.Add("rawSizeBytes", "4096‬"); // the uncompressed size is 4096 bytes
blob.Metadata.Add("kustoIgnoreFirstRecord", "true"); // First line of this csv file are headers
blob.UploadFromFile(csvCompressedLocalFileName);
```

## <a name="blob-lifecycle"></a>Blob 生命周期

Azure 数据资源管理器在引入后不会删除 blob。 请使用 [Azure Blob 存储生命周期](/azure/storage/blobs/storage-lifecycle-management-concepts?tabs=azure-portal)来管理你的 blob 删除。 建议将 blob 保留三到五天。

## <a name="known-issues"></a>已知问题

使用 Azure 数据资源管理器[导出](../data-export/export-data-to-storage.md)用于事件网格引入的文件时，请注意： 
* 如果提供给导出命令的连接字符串或提供给[外部表](../data-export/export-data-to-an-external-table.md)的连接字符串是 [ADLS Gen2 格式](../../api/connection-strings/storage.md#azure-data-lake-store)的连接字符串（例如 `abfss://filesystem@accountname.dfs.core.chinacloudapi.cn`），但没有为分层命名空间启用存储帐户，则不会触发事件网格通知。 
* 如果没有为分层命名空间启用该帐户，则连接字符串必须使用 [Blob 存储](../../api/connection-strings/storage.md#azure-storage-blob)格式（例如 `https://accountname.blob.core.chinacloudapi.cn`）。 即使使用 ADLS Gen2 连接字符串，导出也可以按预期工作，但是不会触发通知，并且事件网格引入也不会进行。
