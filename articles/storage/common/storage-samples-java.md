---
title: 使用 Java 的 Azure 存储示例 | Microsoft 文档
description: 查看、下载和运行 Azure 存储的示例代码和应用程序。 使用 Java 存储客户端库发现 blob、队列、表和文件的入门示例。
author: WenJason
ms.author: v-jay
origin.date: 09/06/2019
ms.date: 02/10/2020
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: 3a6aa72900614d743c4ebc0b12410915e89bb766
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028995"
---
# <a name="azure-storage-samples-using-java"></a>使用 Java 的 Azure 存储示例

下表概述了我们的示例存储库以及每个示例中介绍的方案。 单击链接可查看 GitHub 中相应的示例代码。

> [!NOTE]
> 这些示例使用 Azure 存储 Java v11 库。 有关 v12 代码，请参阅 GitHub 存储库中的[示例](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-blob/src/samples/java/com/azure/storage/blob)。

## <a name="blob-samples-v11"></a>Blob 示例 (v11)

| **方案** | **示例代码** |
|--------------|-----------------|
| 追加 Blob | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| 块 blob | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| 客户端加密 | [Java 中的 Azure 客户端加密入门](https://github.com/Azure-Samples/storage-java-client-side-encryption) |
| 复制 Blob | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| 创建容器 | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| 删除 Blob | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| 删除容器 | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| Blob 元数据/属性/统计信息 | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java) |
| 容器 ACL/元数据/属性 | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java) |
| 获取页面范围 | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java#L399) |
| 租用 Blob/容器 | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| 列出 Blob/容器 | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| 页 blob | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |
| SAS | [SAS 测试示例](https://github.com/Azure/azure-storage-java/blob/89540f018f1160ce55619c6fe7b5f5ff57d0ce10/src/test/java/com/microsoft/azure/storage/Samples.java#L513) |
| 服务属性 | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java) |
| 快照 Blob | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) |

## <a name="file-samples-v11"></a>文件示例 (v11)

| **方案** | **示例代码** |
|--------------|-----------------|
| 创建共享/目录/文件 | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| 删除共享/目录/文件 | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| 目录属性/元数据 | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) |
| 下载文件 | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| 文件属性/元数据/指标 | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) |
| 文件服务属性 | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) |
| 列出目录和文件 | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| 列出共享 | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) |
| 共享属性/元数据/统计信息 | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) |

## <a name="queue-samples-v11"></a>队列示例 (v11)

| **方案** | **示例代码** |
|--------------|-----------------|
| 添加消息 | [Getting Started with Azure Queue Service in Java](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java#L63) |
| 客户端加密 | [Java 中的 Azure 客户端加密入门](https://github.com/Azure-Samples/storage-java-client-side-encryption/blob/master/src/gettingstarted/KeyVaultGettingStarted.java) |
| 创建队列 | [Getting Started with Azure Queue Service in Java](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java) |
| 删除消息/队列 | [Getting Started with Azure Queue Service in Java](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java) |
| 速览消息 | [Getting Started with Azure Queue Service in Java](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java) |
| 队列 ACL/元数据/统计信息 | [Getting Started with Azure Queue Service in Java](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java) |
| 队列服务属性 | [Getting Started with Azure Queue Service in Java](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java) |
| 更新消息 | [Getting Started with Azure Queue Service in Java](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java)
|
## <a name="table-samples-v11"></a>表示例 (v11)

| **方案** | **示例代码** |
|--------------|-----------------|
| 创建表 | [Getting Started with Azure Table Service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| 删除实体/表 | [Getting Started with Azure Table Service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| 插入/合并/替换实体 | [Getting Started with Azure Table Service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| 查询实体 | [Getting Started with Azure Table Service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| 查询表 | [Getting Started with Azure Table Service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
| 表 ACL/属性 | [Getting Started with Azure Table Service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableAdvanced.java) |
| 更新实体 | [Getting Started with Azure Table Service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) |
## <a name="azure-code-samples-library"></a>Azure 代码示例库

若要查看完整的示例库，请访问 [Azure 代码示例](https://azure.microsoft.com/resources/samples/?service=storage) 库，其中包括 Azure 存储的示例，可以下载并在本地运行。 代码示例库提供 .zip 格式的示例代码。 此外，用户也可以浏览 GitHub 存储库以获取每个示例并进行克隆。

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>入门指南

有关 Azure 存储客户端库的安装和入门说明，请查看以下指南。

* [Java 中的 Azure Blob 服务入门](../blobs/storage-quickstart-blobs-java.md)
* [Java 中的 Azure 队列服务入门](../queues/storage-java-how-to-use-queue-storage.md)
* [Java 中的 Azure 表服务入门](../../cosmos-db/table-storage-how-to-use-java.md)
* [Java 中的 Azure 文件服务入门](../files/storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>后续步骤

了解其他语言的示例：

* .NET:[使用 .NET 的 Azure 存储示例](storage-samples-dotnet.md)
* JavaScript/Node.js：[使用 JavaScript 的 Azure 存储示例](storage-samples-javascript.md)
* Python:[使用 Python 的 Azure 存储示例](storage-samples-python.md)
* 所有其他语言：[Azure 存储示例](storage-samples.md)
