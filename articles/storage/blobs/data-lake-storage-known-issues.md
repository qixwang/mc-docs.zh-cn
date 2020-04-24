---
title: Azure Data Lake Storage Gen2 的已知问题 | Microsoft Docs
description: 了解 Azure Data Lake Storage Gen2 的限制和已知问题。
author: WenJason
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
origin.date: 02/25/2020
ms.date: 03/30/2020
ms.author: v-jay
ms.reviewer: jamesbak
ms.openlocfilehash: 9cef96ffc390984787396967a10313db84832491
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80290432"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2 的已知问题

本文介绍 Azure Data Lake Storage Gen2 的限制和已知问题。

## <a name="supported-blob-storage-features"></a>支持的 Blob 存储功能

越来越多的 Blob 存储功能现在兼容有分层命名空间的帐户。 如需完整列表，请参阅 [Azure Data Lake Storage Gen2 中可用的 Blob 存储功能](data-lake-storage-supported-blob-storage-features.md)。

## <a name="supported-azure-service-integrations"></a>支持的 Azure 服务集成

Data Lake Storage gen2 支持多个可用于引入数据、执行分析和创建可视化表示形式的 Azure 服务。 有关受支持的 Azure 服务的列表，请参阅[支持 Azure Data Lake Storage Gen2 的 Azure 服务](data-lake-storage-supported-azure-services.md)。

请参阅[支持 Azure Data Lake Storage Gen2 的 Azure 服务](data-lake-storage-supported-azure-services.md)。

## <a name="supported-open-source-platforms"></a>支持的开源平台

多个开源平台支持 Data Lake Storage Gen2。 有关完整列表，请参阅[支持 Azure Data Lake Storage Gen2 的开源平台](data-lake-storage-supported-open-source-platforms.md)。

请参阅[支持 Azure Data Lake Storage Gen2 的开源平台](data-lake-storage-supported-open-source-platforms.md)。

## <a name="blob-storage-apis"></a>Blob 存储 API

Blob API 和 Data Lake Storage Gen2 API 可以对相同的数据执行操作。

本部分介绍使用 Blob API 和 Data Lake Storage Gen2 API 对相同的数据执行操作时存在的问题和限制。

* 不能同时使用 Blob API 和 Data Lake Storage API 向文件的同一实例写入数据。 如果使用 Data Lake Storage Gen2 API 向某个文件写入数据，则在调用[获取块列表](https://docs.microsoft.com/rest/api/storageservices/get-block-list) Blob API 时，该文件的块将不可见。 覆盖某个文件时，可以使用 Data Lake Storage Gen2 API 或 Blob API。 这不会影响文件属性。

* 如果在使用[列出 Blob](https://docs.microsoft.com/rest/api/storageservices/list-blobs) 操作时不指定分隔符，则结果会包含目录和 Blob。 如果选择使用分隔符，请只使用正斜杠 (`/`)。 这是唯一支持的分隔符。

* 如果使用[删除 Blob](https://docs.microsoft.com/rest/api/storageservices/delete-blob) API 来删除目录，则只能在该目录为空的情况下将其删除。 这意味着，不能使用 Blob API 以递归方式删除目录。

这些 Blob REST API 不受支持：

* [放置 Blob（页）](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [放置页](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [获取页面范围](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [增量复制 Blob](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [从 URL 放置页](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [放置 Blob（追加）](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [追加块](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [从 URL 追加块](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

在有分层命名空间的帐户中，非托管 VM 磁盘不受支持。 若要在存储帐户中启用分层命名空间，请将非托管 VM 磁盘放到未启用分层命名空间功能的存储帐户中。

<a id="api-scope-data-lake-client-library" />

## <a name="file-system-support-in-sdks"></a>SDK 中的文件系统支持

- [.NET](data-lake-storage-directory-file-acl-dotnet.md)、[Java](data-lake-storage-directory-file-acl-java.md)、[Python](data-lake-storage-directory-file-acl-python.md) 和 [JavaScript](data-lake-storage-directory-file-acl-javascript.md) 支持以公共预览版提供。 目前不支持其他 SDK。
- 获取和设置 ACL 的操作当前不是递归的。

## <a name="filesystem-support-in-powershelli"></a>PowerShell 中的文件系统支持

- [PowerShell](data-lake-storage-directory-file-acl-powershell.md) 支持以公共预览版提供。
- 获取和设置 ACL 的操作当前不是递归的。

## <a name="lifecycle-management-policies"></a>生命周期管理策略

* 尚不支持删除 Blob 快照的功能。  

* 当前有一些影响生命周期管理策略和存档访问层的 Bug。 

## <a name="diagnostic-logs"></a>诊断日志

Azure 存储资源管理器 1.10.x 不能用于查看诊断日志。 若要查看日志，请使用 AzCopy 或 SDK。

## <a name="blobfuse"></a>Blobfuse

不支持 Blobfuse。



<a id="known-issues-tools" />

## <a name="azcopy"></a>AzCopy

仅使用最新版本的 AzCopy ([AzCopy v10](/storage/common/storage-use-azcopy-v10?toc=%2fstorage%2ftables%2ftoc.json))。 不支持早期版本的 AzCopy，例如 AzCopy v8.1。

<a id="storage-explorer" />

## <a name="azure-storage-explorer"></a>Azure 存储资源管理器

只使用 `1.6.0` 或更高版本。当前存在影响版本 `1.11.0` 的存储 Bug，在某些情况下可能会导致身份验证错误。 即将推出存储 bug 的修补程序，但我们建议你使用可供`1.10.x`免费下载[的版本 ](/vs-azure-tools-storage-explorer-relnotes) 作为一种解决方法。 `1.10.x` 不受存储 Bug 的影响。

<a id="third-party-apps" />

## <a name="third-party-applications"></a>第三方应用程序

对于使用 REST API 保持正常运行的第三方应用程序，如果将这些应用程序与调用 Blob API 的 Data Lake Storage Gen2 应用程序配合使用，则它们可继续正常运行。

## <a name="access-control-lists-acl-and-anonymous-read-access"></a>访问控制列表 (ACL) 和匿名读取访问

如果已将[匿名读取访问](storage-manage-access-to-resources.md)授予容器，则 ACL 对该容器或该容器中的文件没有影响。

## <a name="windows-azure-storage-blob-wasb-driver"></a>Windows Azure 存储 Blob (WASB) 驱动程序

目前，将 WASB 驱动程序与具有分层命名空间的帐户结合使用时，有几个问题。 建议在工作负荷中使用 [Azure Blob File System (ABFS)](data-lake-storage-abfs-driver.md) 驱动程序。 

