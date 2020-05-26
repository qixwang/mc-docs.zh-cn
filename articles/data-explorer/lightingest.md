---
title: LightIngest 是用于将数据引入 Azure 数据资源管理器的一个命令行实用工具。
description: 了解 LightIngest - 用于将即席数据引入 Azure 数据资源管理器的命令行实用工具。
author: orspod
ms.author: v-tawe
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
origin.date: 03/17/2020
ms.date: 05/09/2020
ms.openlocfilehash: 0c29446215738fab2b78e8909406a223b8092066
ms.sourcegitcommit: bfbd6694da33f703481386f2a3f16850c4e94bfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83417740"
---
# <a name="install-and-use-lightingest"></a>安装和使用 LightIngest

LightIngest 是用于将即席数据引入 Azure 数据资源管理器的命令行实用工具。
该实用工具可以从本地文件夹或 Azure Blob 存储容器提取源数据。

## <a name="prerequisites"></a>先决条件

* LightIngest - 将其作为 [Microsoft.Azure.Kusto.Tools NuGet 包](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Tools/)的一部分下载

    ![Lightingest 下载](media/lightingest/lightingest-download-area.png)

* WinRAR - 从 [www.win-rar.com/download.html](http://www.win-rar.com/download.html) 下载

## <a name="install-lightingest"></a>安装 LightIngest

1. 在计算机上导航到 LightIngest 所下载到的位置。 
1. 使用 WinRAR 将 tools 目录解压缩到计算机上。

## <a name="run-lightingest"></a>运行 LightIngest

1. 在计算机上导航到解压缩的 tools 目录。
1. 从位置栏中删除现有的位置信息。
    
      ![删除位置信息](media/lightingest/lightingest-location-bar.png)

1. 输入 `cmd`，然后按 Enter。
1. 在命令提示符下，输入 `LightIngest.exe`，后接相关的命令行参数。

    > [!Tip]
    > 若要查看支持的命令行参数列表，请输入 `LightIngest.exe /help`。
    >
    >![命令行帮助](media/lightingest/lightingest-cmd-line-help.png)

1. 输入 `ingest-`，后接用于管理引入操作的 Azure 数据资源管理器群集的连接字符串。
    将连接字符串括在双引号中，并遵循 [Kusto 连接字符串规范](https://docs.microsoft.com/azure/data-explorer/kusto/api/connection-strings/kusto)。

    例如：
    ```
    ingest-{Cluster name and region}.kusto.chinacloudapi.cn;AAD Federated Security=True"  -db:{Database} -table:Trips -source:"https://{Account}.blob.core.chinacloudapi.cn/{ROOT_CONTAINER};{StorageAccountKey}" -pattern:"*.csv.gz" -format:csv -limit:2 -ignoreFirst:true -cr:10.0 -dontWait:true
    ```

* 建议的方法是让 `LightIngest` 使用 `https://ingest-{yourClusterNameAndRegion}.kusto.chinacloudapi.cn` 上的引入终结点。 这样，Azure 数据资源管理器服务就可以管理引入负载，而你可以在发生暂时性错误时轻松恢复。 不过，也可以将 `LightIngest` 配置为直接使用引擎终结点 (`https://{yourClusterNameAndRegion}.kusto.chinacloudapi.cn`)。

> [!Note]
> 如果直接通过引擎终结点引入，则不需要包含 `ingest-`，但不会有 DM 功能来保护引擎并提高引入成功率。

* 若要提供最佳引入性能，LightIngest 必须知道原始数据大小，因此，LightIngest 将估算本地文件的非压缩大小。 但如果没有事先下载已压缩的 Blob，LightIngest 可能无法正确估算这些 Blob 的原始大小。 因此，在引入压缩的 Blob 时，请将 Blob 元数据的 `rawSizeBytes` 属性设置为以字节为单位的非压缩数据大小。

## <a name="general-command-line-arguments"></a>常规命令行参数

|参数名称         |短名称   |类型    |必需 |说明                                |
|----------------------|-------------|--------|----------|-------------------------------------------|
|                      |             |string  |必需 |[Azure 数据资源管理器连接字符串](https://docs.microsoft.com/azure/data-explorer/kusto/api/connection-strings/kusto)，指定用于处理引入操作的 Kusto 终结点。 应括在双引号中 |
|-database             |-db          |string  |可选  |目标 Azure 数据资源管理器数据库名称 |
|-table                |             |string  |必需 |目标 Azure 数据资源管理器表名称 |
|-sourcePath           |-source      |string  |必需 |源文件的路径，或 Blob 容器的根 URI。 如果数据位于 Blob 中，则必须包含存储帐户密钥或 SAS。 建议括在双引号中 |
|-prefix               |             |string  |可选  |当要引入的源数据驻留在 Blob 存储中时，此 URL 前缀（不包括容器名称）将由所有 Blob 共享。 <br>例如，如果数据位于 `MyContainer/Dir1/Dir2` 中，则前缀应是 `Dir1/Dir2`。 建议括在双引号中 |
|-pattern              |             |string  |可选  |提取源文件/Blob 时所遵循的模式。 支持通配符。 例如，`"*.csv"`。 建议括在双引号中 |
|-zipPattern           |             |string  |可选  |选择要引入 ZIP 存档中的哪些文件时要使用的正则表达式。<br>该存档中的所有其他文件将被忽略。例如 `"*.csv"`。 建议括在双引号中 |
|-format               |-f           |string  |可选  |源数据格式。 必须是[支持的格式](https://docs.azure.cn/data-explorer/ingestion-supported-formats)之一 |
|-ingestionMappingPath |-mappingPath |string  |可选  |引入列映射文件的路径（对于 Json 和 Avro 格式是必需的）。 请参阅[数据映射](https://docs.microsoft.com/azure/data-explorer/kusto/management/mappings) |
|-ingestionMappingRef  |-mappingRef  |string  |可选  |预先创建的引入列映射的名称（对于 Json 和 Avro 格式是必需的）。 请参阅[数据映射](https://docs.microsoft.com/azure/kusto/management/mappings) |
|-creationTimePattern  |             |string  |可选  |如果设置此参数，它将用于从文件或 Blob 路径中提取 CreationTime 属性。 请参阅[使用 CreationTimePattern 参数](#using-creationtimepattern-argument) |
|-ignoreFirstRow       |-ignoreFirst |bool    |可选  |如果设置此参数，将忽略每个文件/Blob 的第一条记录（例如，如果源数据包含标头） |
|-tag                  |             |string  |可选  |要与引入的数据相关联的[标记](https://docs.microsoft.com/azure/data-explorer/kusto/management/extents-overview#extent-tagging)。 允许多个标记 |
|-dontWait             |             |bool    |可选  |如果设置为“true”，则不等待引入完成。 引入大量文件/Blob 时非常有用 |

### <a name="using-creationtimepattern-argument"></a>使用 CreationTimePattern 参数

`-creationTimePattern` 参数从文件或 Blob 路径中提取 CreationTime 属性。 此模式不需要反映整个项路径，只需反映包含要使用的时间戳的部分。

参数值必须包括：
* 紧靠在时间戳前面的常量测试，括在单引号中
* 时间戳格式，采用标准的 [.NET 日期时间表示法](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings)
* 紧跟在时间戳之后的常量值。 例如，如果 blob 名称以 `historicalvalues19840101.parquet` 结尾（时间戳是表示年份的四位数、表示月份的两位数和表示月份日期的两位数），则 `-creationTimePattern` 参数的相应值为：

```
ingest-{Cluster name and region}.kusto.chinacloudapi.cn;AAD Federated Security=True -db:{Database} -table:Trips -source:"https://{Account}.blob.core.chinacloudapi.cn/{ROOT_CONTAINER};{StorageAccountKey}" -creationTimePattern:"'historicalvalues'yyyyMMdd'.parquet'"
 -pattern:"*.csv.gz" -format:csv -limit:2 -ignoreFirst:true -cr:10.0 -dontWait:true
```

### <a name="command-line-arguments-for-advanced-scenarios"></a>用于高级方案的命令行参数

|参数名称         |短名称   |类型    |必需 |说明                                |
|----------------------|-------------|--------|----------|-------------------------------------------|
|-compression          |-cr          |Double  |可选  |压缩比提示。 引入压缩的文件/Blob 来帮助 Azure 数据资源管理器评估原始数据大小时，此参数非常有用。 计算方式为原始大小除以压缩大小 |
|-limit                |-l           |integer |可选  |如果设置此参数，则会将引入限制为前 N 个文件 |
|-listOnly             |-list        |bool    |可选  |如果设置此参数，仅显示已选择引入的项| 
|-ingestTimeout        |             |integer |可选  |完成所有引入操作的超时（分钟）。 默认为 `60`|
|-forceSync            |             |bool    |可选  |如果设置此参数，将强制同步引入。 默认为 `false` |
|-dataBatchSize        |             |integer |可选  |设置每个引入操作的总大小限制（MB，非压缩） |
|-filesInBatch         |             |integer |可选  |设置每个引入操作的文件/Blob 计数限制 |
|-devTracing           |-trace       |string  |可选  |如果设置此参数，诊断日志将写入本地目录（默认为当前目录中的 `RollingLogs`，可以通过设置开关值修改此位置） |

## <a name="blob-metadata-properties"></a>Blob 元数据属性
与 Azure Blob 配合使用时，LightIngest 将使用特定的 Blob 元数据属性来增强引入过程。

|元数据属性                            | 使用情况                                                                           |
|---------------------------------------------|---------------------------------------------------------------------------------|
|`rawSizeBytes`, `kustoUncompressedSizeBytes` | 如果设置此参数，它将解释为非压缩数据大小                       |
|`kustoCreationTime`, `kustoCreationTimeUtc`  | 解释为 UTC 时间戳。 如果设置此参数，它将用于重写 Kusto 中的创建时间。 在回填方案中非常有用 |

## <a name="usage-examples"></a>用法示例

<!-- Waiting for Tzvia or Vladik to rewrite the instructions for this example before publishing it

### Ingesting a specific number of blobs in JSON format

* Ingest two blobs under a specified storage account {Account}, in `JSON` format matching the pattern `.json`
* Destination is the database {Database}, the table `SampleData`
* Indicate that your data is compressed with the approximate ratio of 10.0
* LightIngest won't wait for the ingestion to be completed

To use the LightIngest command below:
1. Create a table command and enter the table name into the LightIngest command, replacing `SampleData`.
1. Create a mapping command and enter the IngestionMappingRef command, replacing `SampleData_mapping`.
1. Copy your cluster name and enter it into the LightIngest command, replacing `{ClusterandRegion}`.
1. Enter the database name into the LightIngest command, replacing `{Database name}`.
1. Replace `{Account}` with your account name and replace `{ROOT_CONTAINER}?{SAS token}` with the appropriate information.

    ```
    LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.chinacloudapi.cn;Fed=True"  
        -db:{Database name} 
        -table:SampleData 
        -source:"https://{Account}.blob.core.chinacloudapi.cn/{ROOT_CONTAINER}?{SAS token}" 
        -IngestionMappingRef:SampleData_mapping 
        -pattern:"*.json" 
        -format:JSON 
        -limit:2 
        -cr:10.0 
        -dontWait:true
    ```
     
1. In Azure Data Explorer, open query count.

    ![Injestion result in Azure Data Explorer](media/lightingest/lightingest-show-failure-count.png)
-->

### <a name="ingesting-blobs-using-a-storage-account-key-or-a-sas-token"></a>使用存储帐户密钥或 SAS 令牌引入 Blob

* 引入指定存储帐户 `ACCOUNT` 下文件夹 `DIR` 中容器 `CONT` 下的与模式 `*.csv.gz` 匹配的 10 个 Blob
* 目标是数据库 `DB` 表 `TABLE`，引入映射 `MAPPING` 已在目标中预先创建
* 工具将等到引入操作完成
* 请注意，有不同的选项用于指定目标数据库和存储帐户密钥与SAS 令牌

```
LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.chinacloudapi.cn;Fed=True"
  -database:DB
  -table:TABLE
  -source:"https://ACCOUNT.blob.core.chinacloudapi.cn/{ROOT_CONTAINER};{StorageAccountKey}"
  -prefix:"DIR"
  -pattern:*.csv.gz
  -format:csv
  -mappingRef:MAPPING
  -limit:10

LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.chinacloudapi.cn;Fed=True;Initial Catalog=DB"
  -table:TABLE
  -source:"https://ACCOUNT.blob.core.chinacloudapi.cn/{ROOT_CONTAINER}?{SAS token}"
  -prefix:"DIR"
  -pattern:*.csv.gz
  -format:csv
  -mappingRef:MAPPING
  -limit:10
```

### <a name="ingesting-all-blobs-in-a-container-not-including-header-rows"></a>引入容器中的所有 Blob，不包括标头行

* 引入指定存储帐户 `ACCOUNT` 下文件夹 `DIR1/DIR2` 中容器 `CONT` 下的与模式 `*.csv.gz` 匹配的所有 Blob
* 目标是数据库 `DB` 表 `TABLE`，引入映射 `MAPPING` 已在目标中预先创建
* 源 Blob 包含标头行，因此工具被指示删除每个 Blob 的第一条记录
* 工具将发布要引入的数据，且不会等待引入操作完成

```
LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.chinacloudapi.cn;Fed=True"
  -database:DB
  -table:TABLE
  -source:"https://ACCOUNT.blob.core.chinacloudapi.cn/{ROOT_CONTAINER}?{SAS token}"
  -prefix:"DIR1/DIR2"
  -pattern:*.csv.gz
  -format:csv
  -mappingRef:MAPPING
  -ignoreFirstRow:true
```

### <a name="ingesting-all-json-files-from-a-path"></a>从一个路径引入所有 JSON 文件

* 引入路径 `PATH` 下的与模式 `*.json` 匹配的所有文件
* 目标为数据库 `DB` 表 `TABLE`，引入映射已在本地文件 `MAPPING_FILE_PATH` 中定义
* 工具将发布要引入的数据，且不会等待引入操作完成

```
LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.chinacloudapi.cn;Fed=True"
  -database:DB
  -table:TABLE
  -source:"PATH"
  -pattern:*.json
  -format:json
  -mappingPath:"MAPPING_FILE_PATH"
```

### <a name="ingesting-files-and-writing-diagnostic-trace-files"></a>引入文件并写入诊断跟踪文件

* 引入路径 `PATH` 下的与模式 `*.json` 匹配的所有文件
* 目标为数据库 `DB` 表 `TABLE`，引入映射已在本地文件 `MAPPING_FILE_PATH` 中定义
* 工具将发布要引入的数据，且不会等待引入操作完成
* 诊断跟踪文件将在本地写入到文件夹 `LOGS_PATH` 下

```
LightIngest.exe "https://ingest-{ClusterAndRegion}.kusto.chinacloudapi.cn;Fed=True"
  -database:DB
  -table:TABLE
  -source:"PATH"
  -pattern:*.json
  -format:json
  -mappingPath:"MAPPING_FILE_PATH"
  -trace:"LOGS_PATH"
```
## <a name="changelog"></a>更改日志
|版本        |更改                                                                             |
|---------------|------------------------------------------------------------------------------------|
|4.0.9.0        |<ul><li>添加了 `-zipPattern` 参数</li><li>添加了 `-listOnly` 参数</li><li>在开始运行之前显示参数摘要</li><li>根据 `-creationTimePattern` 参数，从 Blob 元数据属性或者从 Blob 或文件名读取 CreationTime</li></ul>|
