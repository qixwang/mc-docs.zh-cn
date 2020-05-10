---
title: 在 Azure Data Lake Storage Gen2 中复制和转换数据
description: 了解如何使用 Azure 数据工厂向/从 Azure Data Lake Storage Gen2 复制数据，以及如何在 Azure Data Lake Storage Gen2 中转换数据。
services: data-factory
ms.author: v-jay
author: WenJason
manager: digimobile
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
origin.date: 04/08/2020
ms.date: 05/11/2020
ms.openlocfilehash: 8400e9ace87a23974cee2fdcd13767df5f0d5bb4
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82197863"
---
# <a name="copy-and-transform-data-in-azure-data-lake-storage-gen2-using-azure-data-factory"></a>使用 Azure 数据工厂在 Azure Data Lake Storage Gen2 中复制和转换数据

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Lake Storage Gen2 (ADLS Gen2) 是一组专用于大数据分析的功能，内置于 [Azure Blob 存储](../storage/blobs/storage-blobs-introduction.md)中。 它可使用文件系统和对象存储范例与数据进行交互。

本文概述了如何使用 Azure 数据工厂中的复制活动从/向 Azure Data Lake Storage Gen2 复制数据。 若要了解 Azure 数据工厂，请阅读[介绍性文章](introduction.md)。

>[!TIP]
>对于数据湖或数据仓库迁移方案，请从[使用 Azure 数据工厂将数据从数据湖或数据仓库迁移到 Azure](data-migration-guidance-overview.md) 了解更多信息。

## <a name="supported-capabilities"></a>支持的功能

此 Azure Data Lake Storage Gen2 连接器支持以下活动：

- 带有[支持的源或接收器矩阵](copy-activity-overview.md)的[复制活动](copy-activity-overview.md)
- [Lookup 活动](control-flow-lookup-activity.md)
- [GetMetadata 活动](control-flow-get-metadata-activity.md)
- [Delete 活动](delete-activity.md)

对于复制活动，使用此连接器可以：

- 通过使用帐户密钥、服务主体或托管标识进行 Azure 资源身份验证来从/向 Azure Data Lake Storage Gen2 复制数据。
- 按原样复制文件，或使用[支持的文件格式和压缩编解码器](supported-file-formats-and-compression-codecs.md)分析/生成文件。
- [在复制期间保留文件元数据](#preserve-metadata-during-copy)。

>[!IMPORTANT]
>如果在 Azure 存储防火墙设置上启用了“允许信任的 Microsoft 服务访问此存储帐户”  选项，并且要使用 Azure 集成运行时连接到 Data Lake Storage Gen2，则必须对 ADLS Gen2 使用[托管标识身份验证](#managed-identity)。


## <a name="get-started"></a>入门

>[!TIP]
>有关使用 Data Lake Storage Gen2 连接器的演练，请参阅[将数据加载到 Azure Data Lake Storage Gen2](load-azure-data-lake-storage-gen2.md)。

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

以下部分介绍了用于定义 Data Lake Storage Gen2 特定的数据工厂实体的属性。

## <a name="linked-service-properties"></a>链接服务属性

Azure Data Lake Storage Gen2 连接器支持以下身份验证类型。 请参阅相应部分的了解详细信息：

- [帐户密钥身份验证](#account-key-authentication)
- [服务主体身份验证](#service-principal-authentication)
- [Azure 资源的托管标识身份验证](#managed-identity)

>[!NOTE]
>使用 PolyBase 将数据载入 SQL 数据仓库时，如果源 Data Lake Storage Gen2 中未配置虚拟网络终结点，则必须使用 PolyBase 所需的托管标识身份验证。 请参阅[托管标识身份验证](#managed-identity)部分，其中提供了更多配置先决条件。

### <a name="account-key-authentication"></a>帐户密钥身份验证

若要使用存储帐户密钥身份验证，需支持以下属性：

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | type 属性必须设置为 AzureBlobFS  。 |是 |
| url | Data Lake Storage Gen2 的终结点，其模式为 `https://<accountname>.dfs.core.chinacloudapi.cn`。 | 是 | 
| accountKey | Data Lake Storage Gen2 的帐户密钥 将此字段标记为 SecureString 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。 |是 |
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 可使用 Azure Integration Runtime 或自承载集成运行时（如果数据存储位于专用网络）。 如果未指定此属性，则使用默认的 Azure Integration Runtime。 |否 |

>[!NOTE]
>使用帐户密钥身份验证时，不支持辅助 ADLS 文件系统终结点。 可以使用其他身份验证类型。

**示例：**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.chinacloudapi.cn", 
            "accountkey": { 
                "type": "SecureString", 
                "value": "<accountkey>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>服务主体身份验证

若要使用服务主体身份验证，请执行以下步骤。

1. 遵循[将应用程序注册到 Azure AD 租户](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)，在 Azure Active Directory (Azure AD) 中注册一个应用程序实体。 记下下面的值，这些值用于定义链接服务：

    - 应用程序 ID
    - 应用程序密钥
    - 租户 ID

2. 向服务主体授予适当的权限。 查看[文件和目录上的访问控制列表](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories)中的一些示例，来了解 Data Lake Storage Gen2 中的权限的工作原理。

    - **作为源**：在存储资源管理器中，至少授予对所有上游文件夹和文件系统的“执行”权限，并授予对文件的“读取”权限以进行复制   。 或者，在访问控制 (IAM) 中至少授予“存储 Blob 数据读取者”角色  。
    - **作为接收器**：在存储资源管理器中，至少授予对所有上游文件夹和文件系统的“执行”权限，并授予对接收器文件夹的“写入”权限   。 或者，在访问控制 (IAM) 中至少授予“存储 Blob 数据参与者”角色  。

>[!NOTE]
>如果使用数据工厂 UI 进行创作，并且服务主体未在 IAM 中设置为“存储 Blob 数据读取者/参与者”角色，则在执行测试连接或在文件夹中浏览/导航时，请选择“测试到文件路径的连接”或“从指定路径浏览”，然后指定具有“读取 + 执行”  权限的路径以继续。

链接服务支持以下属性：

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | type 属性必须设置为 AzureBlobFS  。 |是 |
| url | Data Lake Storage Gen2 的终结点，其模式为 `https://<accountname>.dfs.core.chinacloudapi.cn`。 | 是 | 
| servicePrincipalId | 指定应用程序的客户端 ID。 | 是 |
| servicePrincipalKey | 指定应用程序的密钥。 将此字段标记为 `SecureString`，以便安全地将其存储在数据工厂中。 或者，可以[引用 Azure Key Vault 中存储的机密](store-credentials-in-key-vault.md)。 | 是 |
| tenant | 指定应用程序的租户信息（域名或租户 ID）。 将鼠标悬停在 Azure 门户右上角进行检索。 | 是 |
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 可使用 Azure Integration Runtime 或自承载集成运行时（如果数据存储位于专用网络）。 如果未指定，则使用默认 Azure Integration Runtime。 |否 |

**示例：**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.chinacloudapi.cn", 
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.partner.onmschina.cn>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a> Azure 资源的托管标识身份验证

可将数据工厂与代表此特定数据工厂的 [Azure 资源托管标识](data-factory-service-identity.md)相关联。 可以像使用自己的服务主体一样，直接使用此托管标识进行 Data Lake Storage Gen2 身份验证。 此指定工厂可通过此方法访问以及向/从 Data Lake Storage Gen2 复制数据。

若要使用 Azure 资源的托管标识身份验证，请执行以下步骤。

1. 通过复制与工厂一起生成的**托管标识对象 ID** 的值，[检索数据工厂托管标识信息](data-factory-service-identity.md#retrieve-managed-identity)。

2. 向托管标识授予适当的权限。 查看[文件和目录上的访问控制列表](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories)中的一些示例，来了解 Data Lake Storage Gen2 中的权限的工作原理。

    - **作为源**：在存储资源管理器中，至少授予对所有上游文件夹和文件系统的“执行”权限，并授予对文件的“读取”权限以进行复制   。 或者，在访问控制 (IAM) 中至少授予“存储 Blob 数据读取者”角色  。
    - **作为接收器**：在存储资源管理器中，至少授予对所有上游文件夹和文件系统的“执行”权限，并授予对接收器文件夹的“写入”权限   。 或者，在访问控制 (IAM) 中至少授予“存储 Blob 数据参与者”角色  。

>[!NOTE]
>如果使用数据工厂 UI 进行创作，并且托管标识未在 IAM 中设置为“存储 Blob 数据读取者/参与者”角色，则在执行测试连接或在文件夹中浏览/导航时，请选择“测试到文件路径的连接”或“从指定路径浏览”，然后指定具有“读取 + 执行”  权限的路径以继续。

>[!IMPORTANT]
>如果在使用 Data Lake Storage Gen2 托管标识身份验证时使用 PolyBase 将 Data Lake Storage Gen2 中的数据载入 SQL 数据仓库，请确保同时遵循[此指南](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)中的步骤 1 和 2，以便 1) 将 SQL 数据库服务器注册到 Azure Active Directory (Azure AD)，2) 将存储 Blob 数据参与者角色分配到 SQL 数据库服务器；剩余的任务将由数据工厂处理。 如果源 Data Lake Storage Gen2 中已配置 Azure 虚拟网络终结点，若要使用 PolyBase 从中加载数据，必须使用 PolyBase 所需的托管标识身份验证。

链接服务支持以下属性：

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | type 属性必须设置为 AzureBlobFS  。 |是 |
| url | Data Lake Storage Gen2 的终结点，其模式为 `https://<accountname>.dfs.core.chinacloudapi.cn`。 | 是 | 
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 可使用 Azure Integration Runtime 或自承载集成运行时（如果数据存储位于专用网络）。 如果未指定，则使用默认 Azure Integration Runtime。 |否 |

**示例：**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.chinacloudapi.cn", 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各个部分和属性的完整列表，请参阅[数据集](concepts-datasets-linked-services.md)。

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

基于格式的数据集中 `location` 设置下的 Data Lake Storage Gen2 支持以下属性：

| 属性   | 说明                                                  | 必须 |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | 数据集中 `location` 下的 type 属性必须设置为 **AzureBlobFSLocation**。 | 是      |
| fileSystem | Data Lake Storage Gen2 文件系统名称。                              | 否       |
| folderPath | 给定文件系统下的文件夹路径。 如果要使用通配符筛选文件夹，请跳过此设置并在活动源设置中指定。 | 否       |
| fileName   | 给定 fileSystem + folderPath 下的文件名。 如果要使用通配符筛选文件，请跳过此设置并在活动源设置中指定。 | 否       |

**示例：**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobFSLocation",
                "fileSystem": "filesystemname",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>复制活动属性

要完整了解可供活动定义使用的各个部分和属性，请参阅[复制活动配置](copy-activity-overview.md#configuration)和[管道和活动](concepts-pipelines-activities.md)。 本部分列出了 Data Lake Storage Gen2 源和接收器支持的属性。

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Azure Data Lake Storage Gen2 作为源类型

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

基于格式的复制源中的 `storeSettings` 设置下的 Data Lake Storage Gen2 支持以下属性：

| 属性                 | 说明                                                  | 必须                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | `storeSettings` 下的 type 属性必须设置为 **AzureBlobFSReadSettings**。 | 是                                           |
| recursive                | 指示是要从子文件夹中以递归方式读取数据，还是只从指定的文件夹中读取数据。 当 recursive 设置为 true 且接收器是基于文件的存储时，将不会在接收器上复制或创建空的文件夹或子文件夹。 允许的值为 **true**（默认值）和 **false**。 | 否                                            |
| wildcardFolderPath       | 数据集中配置的给定文件系统下包含通配符的文件夹路径，用于筛选源文件夹。 <br>允许的通配符为：`*`（匹配零个或更多字符）和 `?`（匹配零个或单个字符）。 如果实际文件夹名内具有通配符或此转义符，请使用 `^` 进行转义。 <br>请参阅[文件夹和文件筛选器示例](#folder-and-file-filter-examples)中的更多示例。 | 否                                            |
| wildcardFileName         | 给定的文件系统 + folderPath/wildcardFolderPath 下包含通配符的文件名，用于筛选源文件。 <br>允许的通配符为：`*`（匹配零个或更多字符）和 `?`（匹配零个或单个字符）。 如果实际文件夹名内具有通配符或此转义符，请使用 `^` 进行转义。 请参阅[文件夹和文件筛选器示例](#folder-and-file-filter-examples)中的更多示例。 | 如果数据集中未指定 `fileName`，则为“是” |
| modifiedDatetimeStart    | 基于属性“上次修改时间”的文件筛选器。 如果文件的上次修改时间在 `modifiedDatetimeStart` 和 `modifiedDatetimeEnd` 之间的时间范围内，则将选中这些文件。 该时间应用于 UTC 时区，格式为“2018-12-01T05:00:00Z”。 <br> 属性可以为 NULL，这意味着不向数据集应用任何文件特性筛选器。 如果 `modifiedDatetimeStart` 具有日期/时间值，但 `modifiedDatetimeEnd` 为 NULL，则意味着将选中“上次修改时间”属性大于或等于该日期/时间值的文件。 如果 `modifiedDatetimeEnd` 具有日期/时间值，但 `modifiedDatetimeStart` 为 NULL，则意味着将选中“上次修改时间”属性小于该日期/时间值的文件。 | 否                                            |
| modifiedDatetimeEnd      | 同上。                                               | 否                                            |
| maxConcurrentConnections | 可以同时连接到存储库的连接数。 仅在要限制与数据存储的并发连接时指定。 | 否                                            |

**示例：**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureBlobFSReadSettings",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-storage-gen2-as-a-sink-type"></a>Azure Data Lake Storage Gen2 作为接收器类型

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

基于格式的复制接收器中的 `storeSettings` 设置下的 Data Lake Storage Gen2 支持以下属性：

| 属性                 | 说明                                                  | 必须 |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | `storeSettings` 下的 type 属性必须设置为 **AzureBlobFSWriteSettings**。 | 是      |
| copyBehavior             | 定义以基于文件的数据存储中的文件为源时的复制行为。<br/><br/>允许值包括：<br/><b>- PreserveHierarchy（默认）</b>：将文件层次结构保留到目标文件夹中。 指向源文件夹的源文件相对路径与指向目标文件夹的目标文件相对路径相同。<br/><b>- FlattenHierarchy</b>：源文件夹中的所有文件都位于目标文件夹的第一级中。 目标文件具有自动生成的名称。 <br/><b>- MergeFiles</b>：将源文件夹中的所有文件合并到一个文件中。 如果指定了文件名，则合并文件的名称为指定名称。 否则，它是自动生成的文件名。 | 否       |
| blockSizeInMB | 指定用于将数据写入 ADLS Gen2 的块大小（以 MB 为单位）。 详细了解[块 Blob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs)。 <br/>允许值为 **4 到 100 MB**。 <br/>默认情况下，ADF 会根据源存储类型和数据自动确定块大小。 对于以非二进制格式复制到 ADLS Gen2，默认块大小为 100 MB，这样，最多可达 4.95 TB 的数据便可以容纳它。 当数据不大时，它可能并非最优，特别是当你在网络状况不佳的情况下使用自承载集成运行时的时候，这会导致操作超时或性能问题。 可以显式指定块大小，同时确保 blockSizeInMB*50000 足以存储数据，否则复制活动运行将失败。 | 否 |
| maxConcurrentConnections | 可以同时连接到数据存储的连接数。 仅在要限制与数据存储的并发连接时指定。 | 否       |

**示例：**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureBlobFSWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>文件夹和文件筛选器示例

本部分介绍使用通配符筛选器生成文件夹路径和文件名的行为。

| folderPath | fileName | recursive | 源文件夹结构和筛选器结果（用**粗体**表示的文件已检索）|
|:--- |:--- |:--- |:--- |
| `Folder*` | （为空，使用默认值） | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | （为空，使用默认值） | 是 | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | 是 | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>一些 recursive 和 copyBehavior 示例

本节介绍了将 recursive 和 copyBehavior 值进行不同组合所产生的复制操作行为。

| recursive | copyBehavior | 源文件夹结构 | 生成目标 |
|:--- |:--- |:--- |:--- |
| 是 |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用与源相同的结构创建目标 Folder1：<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| 是 |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标 Folder1： <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 的自动生成的名称 |
| 是 |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标 Folder1： <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File5 的内容将合并到一个文件中，且自动生成文件名。 |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标 Folder1： <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>不会选取带有 File3、File4 和 File5 的 Subfolder1。 |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标 Folder1： <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 的自动生成的名称<br/><br/>不会选取带有 File3、File4 和 File5 的 Subfolder1。 |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标 Folder1： <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 的内容将合并到一个文件中，且自动生成文件名。 File1 的自动生成的名称<br/><br/>不会选取带有 File3、File4 和 File5 的 Subfolder1。 |

## <a name="preserve-metadata-during-copy"></a>在复制期间保留元数据

将文件从 Amazon S3/Azure Blob/Azure Data Lake Storage Gen2 复制到 Azure Data Lake Storage Gen2/Azure Blob 时，可以选择保留文件元数据和数据。 从[保留元数据](copy-activity-preserve-metadata.md#preserve-metadata)中了解更多信息。

## <a name="lookup-activity-properties"></a>查找活动属性

若要了解有关属性的详细信息，请查看 [Lookup 活动](control-flow-lookup-activity.md)。

## <a name="getmetadata-activity-properties"></a>GetMetadata 活动属性

若要了解有关属性的详细信息，请查看 [GetMetadata 活动](control-flow-get-metadata-activity.md) 

## <a name="delete-activity-properties"></a>Delete 活动属性

若要了解有关属性的详细信息，请查看 [Delete 活动](delete-activity.md)

## <a name="legacy-models"></a>旧模型

>[!NOTE]
>仍按原样支持以下模型，以实现向后兼容性。 建议你以后使用前面部分中提到的新模型，ADF 创作 UI 已经切换到生成新模型。

### <a name="legacy-dataset-model"></a>旧数据集模型

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | 数据集的 type 属性必须设置为 AzureBlobFSFile  。 |是 |
| folderPath | Data Lake Storage Gen2 中的文件夹的路径。 如果未指定，它指向根目录。 <br/><br/>支持通配符筛选器。 允许的通配符为：`*`（匹配零个或更多字符）和 `?`（匹配零个或单个字符）。 如果实际文件夹名内具有通配符或此转义符，请使用 `^` 进行转义。 <br/><br/>示例：filesystem/folder/。 请参阅[文件夹和文件筛选器示例](#folder-and-file-filter-examples)中的更多示例。 |否 |
| fileName | 指定“folderPath”下的文件的“名称或通配符筛选器”。 如果没有为此属性指定任何值，则数据集会指向文件夹中的所有文件。 <br/><br/>对于筛选器，允许的通配符为：`*`（匹配零个或更多字符）和 `?`（匹配零个或单个字符）。<br/>- 示例 1：`"fileName": "*.csv"`<br/>- 示例 2：`"fileName": "???20180427.txt"`<br/>如果实际文件名内具有通配符或此转义符，请使用 `^` 进行转义。<br/><br/>如果没有为输出数据集指定 fileName，并且没有在活动接收器中指定 preserveHierarchy，则复制活动会自动生成采用以下模式的文件名称  ：“*Data.[activity run ID GUID].[GUID if FlattenHierarchy].[format if configured].[compression if configured]* ”，例如“Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz”。 如果使用表名称而不是查询从表格源进行复制，则名称模式为“[table name].[format].[compression if configured]”，例如“MyTable.csv”。  |否 |
| modifiedDatetimeStart | 基于属性“上次修改时间”的文件筛选器。 如果文件的上次修改时间在 `modifiedDatetimeStart` 和 `modifiedDatetimeEnd` 之间的时间范围内，则将选中这些文件。 该时间应用于 UTC 时区，格式为“2018-12-01T05:00:00Z”。 <br/><br/> 当你要从大量文件中进行文件筛选时，启用此设置将影响数据移动的整体性能。 <br/><br/> 属性可以为 NULL，这意味着不向数据集应用任何文件特性筛选器。 如果 `modifiedDatetimeStart` 具有日期/时间值，但 `modifiedDatetimeEnd` 为 NULL，则意味着将选中“上次修改时间”属性大于或等于该日期/时间值的文件。 如果 `modifiedDatetimeEnd` 具有日期/时间值，但 `modifiedDatetimeStart` 为 NULL，则意味着将选中“上次修改时间”属性小于该日期/时间值的文件。| 否 |
| modifiedDatetimeEnd | 基于属性“上次修改时间”的文件筛选器。 如果文件的上次修改时间在 `modifiedDatetimeStart` 和 `modifiedDatetimeEnd` 之间的时间范围内，则将选中这些文件。 该时间应用于 UTC 时区，格式为“2018-12-01T05:00:00Z”。 <br/><br/> 当你要从大量文件中进行文件筛选时，启用此设置将影响数据移动的整体性能。 <br/><br/> 属性可以为 NULL，这意味着不向数据集应用任何文件特性筛选器。 如果 `modifiedDatetimeStart` 具有日期/时间值，但 `modifiedDatetimeEnd` 为 NULL，则意味着将选中“上次修改时间”属性大于或等于该日期/时间值的文件。 如果 `modifiedDatetimeEnd` 具有日期/时间值，但 `modifiedDatetimeStart` 为 NULL，则意味着将选中“上次修改时间”属性小于该日期/时间值的文件。| 否 |
| format | 若要在基于文件的存储之间按原样复制文件（二进制副本），可以在输入和输出数据集定义中跳过格式节。<br/><br/>若要分析或生成具有特定格式的文件，以下是受支持的文件格式类型：TextFormat、JsonFormat、AvroFormat、OrcFormat 和 ParquetFormat      。 请将 **format** 中的 **type** 属性设置为上述值之一。 有关详细信息，请参阅[文本格式](supported-file-formats-and-compression-codecs-legacy.md#text-format)、[JSON 格式](supported-file-formats-and-compression-codecs-legacy.md#json-format)、[Avro 格式](supported-file-formats-and-compression-codecs-legacy.md#avro-format)、[ORC 格式](supported-file-formats-and-compression-codecs-legacy.md#orc-format)和 [Parquet 格式](supported-file-formats-and-compression-codecs-legacy.md#parquet-format)部分。 |否（仅适用于二进制复制方案） |
| compression | 指定数据的压缩类型和级别。 有关详细信息，请参阅[受支持的文件格式和压缩编解码器](supported-file-formats-and-compression-codecs-legacy.md#compression-support)。<br/>支持的类型为 **GZip**、**Deflate**、**BZip2** 和 **ZipDeflate**。<br/>支持的级别为“最佳”  和“最快”  。 |否 |

>[!TIP]
>如需复制文件夹下的所有文件，请仅指定 **folderPath**。<br>如需复制具有给定名称的单个文件，请指定文件夹部分的 **folderPath** 和文件名部分的 **fileName**。<br>如需复制文件夹下的文件子集，请指定文件夹部分的 **folderPath** 和通配符筛选器部分的 **fileName**。 

**示例：**

```json
{
    "name": "ADLSGen2Dataset",
    "properties": {
        "type": "AzureBlobFSFile",
        "linkedServiceName": {
            "referenceName": "<Azure Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "myfilesystem/myfolder",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>旧复制活动源模型

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | 复制活动源的 type 属性必须设置为 AzureBlobFSSource  。 |是 |
| recursive | 指示是要从子文件夹中以递归方式读取数据，还是只从指定的文件夹中读取数据。 当 recursive 设置为 true 且接收器是基于文件的存储时，将不会在接收器上复制或创建空的文件夹或子文件夹。<br/>允许的值为 **true**（默认值）和 **false**。 | 否 |
| maxConcurrentConnections | 可以同时连接到数据存储的连接数。 仅在要限制与数据存储的并发连接时指定。 | 否 |

**示例：**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen2 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzureBlobFSSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="legacy-copy-activity-sink-model"></a>旧复制活动接收器模型

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | 复制活动接收器的 type 属性必须设置为 AzureBlobFSSink  。 |是 |
| copyBehavior | 定义以基于文件的数据存储中的文件为源时的复制行为。<br/><br/>允许值包括：<br/><b>- PreserveHierarchy（默认）</b>：将文件层次结构保留到目标文件夹中。 指向源文件夹的源文件相对路径与指向目标文件夹的目标文件相对路径相同。<br/><b>- FlattenHierarchy</b>：源文件夹中的所有文件都位于目标文件夹的第一级中。 目标文件具有自动生成的名称。 <br/><b>- MergeFiles</b>：将源文件夹中的所有文件合并到一个文件中。 如果指定了文件名，则合并文件的名称为指定名称。 否则，它是自动生成的文件名。 | 否 |
| maxConcurrentConnections | 可以同时连接到数据存储的连接数。 仅在要限制与数据存储的并发连接时指定。 | 否 |

**示例：**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

## <a name="next-steps"></a>后续步骤

有关数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)。
