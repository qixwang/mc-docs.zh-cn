---
title: Kusto 的 .ingest into 命令（从存储中拉取数据）- Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的 .ingest into 命令（从存储中拉取数据）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/24/2020
ms.date: 07/01/2020
ms.openlocfilehash: 7d9a08232930b2a16ee621d08d6c2248e0085daa
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226199"
---
# <a name="the-ingest-into-command-pull-data-from-storage"></a>.ingest into 命令（从存储中拉取数据）

`.ingest into` 命令通过从一个或多个云存储项目“拉取”数据，将数据引入表中。
例如，此命令可以从 Azure Blob 存储中检索 1000 个 CSV 格式的 blob，对其进行分析，然后将它们一起引入到单个目标表中。
数据将追加到表，不会影响现有记录，也不会修改表的架构。

**语法**

`.ingest` [`async`] `into` `table` *TableName* *SourceDataLocator* [`with` `(` *IngestionPropertyName* `=` *IngestionPropertyValue* [`,` ...] `)`]

**参数**

* `async`：如果指定了此项，命令会立即返回，并继续在后台执行引入操作。 命令的结果将包含一个 `OperationId` 值，该值随后可以与 `.show operation` 命令一起使用，以检索引入的完成状态和结果。
  
* TableName：要将数据引入到其中的表的名称。
  表名在上下文中始终相对于数据库，如果未提供架构映射对象，则其架构是将为数据采用的架构。

* SourceDataLocator：`string` 类型的文本，或由 `(` 和 `)` 字符括起来的以逗号分隔的此类文本列表，表示包含要拉取的数据的存储项目。 请参阅[存储连接字符串](../../api/connection-strings/storage.md)。

> [!NOTE]
> 对于包含实际凭据的 *SourceDataPointer*，强烈建议为其使用[经过模糊处理的字符串文本](../../query/scalar-data-types/string.md#obfuscated-string-literals)。
> 该服务将确保清除其内部跟踪、错误消息和其他位置中的凭据。

* IngestionPropertyName、IngestionPropertyValue：影响引入过程的任意数量的[引入属性](../../../ingestion-properties.md)。

**结果**

命令的结果是一个表，其中包含的记录与命令生成的数据分片（“盘区”）的数量一样多。
如果未生成任何数据分片，则返回一条带有空（零值）盘区 ID 的记录。

|名称       |类型      |说明                                                                |
|-----------|----------|---------------------------------------------------------------------------|
|ExtentId   |`guid`    |该命令生成的数据分片的唯一标识符。|
|ItemLoaded |`string`  |与此记录相关的一个或多个存储项目。             |
|持续时间   |`timespan`|执行引入所花费的时间。                                     |
|HasErrors  |`bool`    |此记录是否表示引入失败。                |
|OperationId|`guid`    |表示操作的唯一 ID。 可以与 `.show operation` 命令一起使用。|

**备注**

此命令不会修改要引入到其中的表的架构。
必要时，数据在引入期间会被“强制纳入”到此架构中，而不采用其他方法（忽略多余的列，将缺少的列视为 null 值）。

**示例**

下一个示例指示引擎将 Azure Blob 存储中的两个 blob 读取为 CSV 文件，并将其内容引入到表 `T` 中。 `...` 表示 Azure 存储共享访问签名 (SAS)，该签名提供对每个 blob 的读取访问权限。 另请注意，使用了经过模糊处理的字符串（字符串值前面的 `h`），以确保从不记录 SAS。

```kusto
.ingest into table T (
    h'https://contoso.blob.core.chinacloudapi.cn/container/file1.csv?...',
    h'https://contoso.blob.core.chinacloudapi.cn/container/file2.csv?...'
)
```

下一个示例用于从 Azure Data Lake Storage Gen 2 (ADLSv2) 引入数据。 此处使用的凭据 (`...`) 是存储帐户凭据（共享密钥），我们只对连接字符串的机密部分使用字符串模糊处理。

```kusto
.ingest into table T (
  'abfss://myfilesystem@contoso.dfs.core.chinacloudapi.cn/path/to/file1.csv;...'
)
```

下一个示例从 Azure Data Lake Storage (ADLS) 引入单个文件。
它使用用户的凭据来访问 ADLS（因此，无需将存储 URI 视为包含机密）。 它还展示了如何指定引入属性。

```kusto
.ingest into table T ('adl://contoso.azuredatalakestore.net/Path/To/File/file1.ext;impersonate')
  with (format='csv')
```

