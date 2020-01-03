---
title: 排查常见的搜索索引器问题
titleSuffix: Azure Cognitive Search
description: 修复 Azure 认知搜索中索引器的错误和常见问题，包括数据源连接、防火墙和丢失的文档。
manager: nitinme
author: mgottein
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.openlocfilehash: 62c6878d2949cb60efc3dc773d9f19220e55c7e4
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336518"
---
# <a name="troubleshooting-common-indexer-issues-in-azure-cognitive-search"></a>排查 Azure 认知搜索中的常见索引器问题

在将数据索引到 Azure 认知搜索中时，索引器可能会遇到许多问题。 故障的主要类别包括：

* [连接到数据源](#data-source-connection-errors)
* [文档处理](#document-processing-errors)
* [目标为索引的文档引入](#index-errors)

## <a name="data-source-connection-errors"></a>数据源连接错误

### <a name="blob-storage"></a>Blob 存储

#### <a name="storage-account-firewall"></a>存储帐户防火墙

Azure 存储提供可配置的防火墙。 默认情况下，防火墙处于禁用状态，因此 Azure 认知搜索可以连接到存储帐户。

防火墙启用后，没有具体的错误消息。 通常情况下，防火墙错误类似于：`The remote server returned an error: (403) Forbidden`。

可以在[门户](https://docs.azure.cn/storage/common/storage-network-security#azure-portal)中验证防火墙是否已启用。 唯一支持的解决方法是通过选择允许从[“所有网络”](https://docs.azure.cn/storage/common/storage-network-security#azure-portal)访问来禁用防火墙。

<!-- If your indexer does not have an attached skillset, you _may_ attempt to [add an exception](https://docs.azure.cn/storage/common/storage-network-security#managing-ip-network-rules) for the IP addresses of your search service. However, this scenario is not supported and is not guaranteed to work. -->

可以通过 ping 搜索服务的 FQDN (`<your-search-service-name>.search.chinacloudapi.cn`) 来查找其 IP 地址。

### <a name="cosmos-db"></a>Cosmos DB

#### <a name="indexing-isnt-enabled"></a>索引未启用

Azure 认知搜索对 Cosmos DB 索引存在隐式依赖。 如果在 Cosmos DB 中关闭自动索引，Azure 认知搜索会返回成功状态，但无法索引容器内容。 有关如何查看设置和启用索引功能的说明，请参阅[管理 Azure Cosmos DB 中的索引编制](https://docs.azure.cn/cosmos-db/how-to-manage-indexing-policy#use-the-azure-portal)。

## <a name="document-processing-errors"></a>文档处理错误

### <a name="unprocessable-or-unsupported-documents"></a>文档无法处理或不受支持

[显式支持可记录格式的 Blob 索引器文档](search-howto-indexing-azure-blob-storage.md#SupportedFormats)。 有时候，Blob 存储容器包含不受支持的文档。 而另一些时候，可能存在有问题的文档。 可以通过[更改配置选项](search-howto-indexing-azure-blob-storage.md#DealingWithErrors)来避免停止这些文档上的索引器：

```
PUT https://[service name].search.chinacloudapi.cn/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false, "failOnUnprocessableDocument" : false } }
}
```

<!-- ### Missing document content -->

## <a name="index-errors"></a>索引错误

### <a name="missing-documents"></a>缺少文档

索引器从[数据源](https://docs.microsoft.com/rest/api/searchservice/create-data-source)查找文档。 有时候，索引中似乎会缺失数据源中本应进行索引的文档。 之所以发生这些错误，有多种常见原因：

* 文档尚未进行索引。 查看门户中是否有成功的索引器运行。
* 文档在索引器运行之后已更新。 如果索引器已在[计划](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-schedule)之中，它最终会重新运行并选取该文档。
* 在数据源中指定的 [query](https://docs.microsoft.com/rest/api/searchservice/create-data-source#request-body-syntax) 排除了该文档。 索引器不能索引不属于数据源的文档。
* [字段映射](https://docs.microsoft.com/rest/api/searchservice/create-indexer#fieldmappings)已更改此文档，因此它看起来不同于预期。
* 使用[查找文档 API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) 来查找文档。
