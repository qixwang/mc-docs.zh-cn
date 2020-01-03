---
title: REST API 版本 2019-05-06-Preview
titleSuffix: Azure Cognitive Search
description: Azure 认知搜索服务 REST API 版本 2019-05-06-Preview 包括试验功能，例如知识存储和客户托管的加密密钥。
manager: nitinme
author: brjohnstmsft
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.openlocfilehash: 9e89432ad8079c53d323a3da4f1d99109dec16ee
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335521"
---
# <a name="azure-cognitive-search-service-rest-api-version-2019-05-06-preview"></a>Azure 认知搜索服务 REST API 版本 2019-05-06-Preview

本文介绍了 `api-version=2019-05-06-Preview` 版的搜索服务 REST API，该版提供尚未正式发布的实验性功能。

> [!NOTE]
> 预览功能可用于目的为收集反馈的测试和试验，可能随时更改。 强烈建议不要在生产应用程序中使用预览版 API。


## <a name="new-in-2019-05-06-preview"></a>2019-05-06-Preview 中的新增功能

<!-- + [Incremental indexing](cognitive-search-incremental-indexing-conceptual.md) is a new mode for indexing that adds state and caching to a skillset, allowing you to reuse existing output when source data, indexer, and skillset definitions are unchanged. This feature applies only to enrichments defined a cognitive skillset. -->

<!-- + [Document Extraction (preview)](cognitive-search-skill-document-extraction.md) is a cognitive skill used during indexing that allows you to extract the contents of a file from within a skillset. Previously, document cracking only occurred prior to skillset execution. With the addition of this skill, you can also perform this operation within skillset execution. -->

<!-- + [Text Translation (preview)](cognitive-search-skill-text-translation.md) is a cognitive skill used during indexing that evaluates text and, for each record, returns the text translated to the specified target language. -->

+ [Cosmos DB 索引器](search-howto-index-cosmosdb.md)支持 MongoDB API、Gremlin API 和 Cassandra API。

+ [Azure Data Lake Storage Gen2 索引器](search-howto-index-azure-data-lake-storage.md)可以为 Data Lake Storage Gen2 中的内容和元数据编制索引。


+ 用于服务端静态加密的[客户托管加密密钥](search-security-manage-encryption-keys.md)也是一项新的预览版功能。 如果你是密钥的唯一所有者，则除了应用 Microsoft 管理的内置静态加密以外，还可以应用一个额外的加密层。

<!-- + [Knowledge store](knowledge-store-concept-intro.md) is a new destination of an AI-based enrichment pipeline. The physical data structure exists in Azure Blob storage and Azure Table storage, and it is created and populated when you run an indexer that has an attached cognitive skillset. The definition of a knowledge store itself is specified within a skillset definition. Within the knowledge store definition, you control the physical structures of your data through *projection* elements that determine how data is shaped, whether data is stored in Table storage or Blob storage, and whether there are multiple views. -->

## <a name="earlier-preview-features"></a>早期预览功能

公共预览版中仍包含早期预览版中发布的功能。 如果你正在调用使用较早预览版 API 版本的 API，可以继续使用该版本或切换到 `2019-05-06-Preview`，而不会对预期行为作出任何更改。

+ [moreLikeThis 查询参数](search-more-like-this.md)查找与特定文档相关的文档。 早期预览版中已有此功能。 

+ [CSV Blob 索引](search-howto-index-csv-blobs.md)每行创建一个文档，而不是每个文本 Blob 创建一个文档。

## <a name="how-to-call-a-preview-api"></a>如何调用预览版 API

早期预览版仍然可用，但随着时间推移会变得过时。 如果代码调用 `api-version=2016-09-01-Preview` 或 `api-version=2017-11-11-Preview`，那些调用仍然有效。 但是，只有最新预览版会获得改进。 

以下示例语法说明了对预览 API 版本的调用。

    GET https://[service name].search.chinacloudapi.cn/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure 认知搜索服务在多个版本内可用。 有关详细信息，请参阅 [API 版本](search-api-versions.md)。

## <a name="next-steps"></a>后续步骤

查看搜索 REST API 参考文档。 如果遇到问题，请通过 [StackOverflow](https://stackoverflow.com/) 向我们寻求帮助，或[联系支持人员](https://support.azure.cn/support/contact)。

> [!div class="nextstepaction"]
> [搜索服务 REST API 参考](https://docs.microsoft.com/rest/api/searchservice/)