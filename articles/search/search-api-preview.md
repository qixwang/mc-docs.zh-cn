---
title: REST API 中的预览版功能
titleSuffix: Azure Cognitive Search
description: Azure 认知搜索服务 REST API 版本 2019-05-06-Preview 包括试验功能，例如，用于实现增量扩充的知识存储和索引器缓存。
manager: nitinme
author: brjohnstmsft
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 02/11/2020
ms.date: 03/02/2020
ms.openlocfilehash: 8f5c4b8d5052d15cf61d6089e6eddb769faa0095
ms.sourcegitcommit: 094c057878de233180ff3b3a3e3c19bc11c81776
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77501428"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Azure 认知搜索中的预览版功能

本文列出了预览版中当前提供的功能。 此列表中未包括已从预览版转变为正式版的功能。 有关正式版的公告，可以查看[服务更新](https://updates.azure.cn)或[新增功能](whats-new.md)。

虽然某些预览版功能在门户和 .NET SDK 中可能可用，但 REST API 始终具有预览版功能。

+ 对于搜索操作，当前预览版版本为 [ **`2019-05-06-Preview`** ](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview)。
+ 对于管理操作，当前预览版版本为 [ **`2019-10-01-Preview`** ](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview)。

<!-- > [!IMPORTANT] -->
<!-- > Preview functionality is provided without a service level agreement, and is not recommended for production workloads. For more information, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). -->

<!-- ## AI enrichment features -->

## <a name="indexing-and-query-features"></a>索引编制和查询功能

预览版搜索 API 中提供了索引器预览版功能。 

|||
|-|-|
| [Cosmos DB 索引器](search-howto-index-cosmosdb.md) | 支持 MongoDB API（预览版）、Gremlin API（预览版）和 Cassandra API（预览版）API 类型。 | 
|  [Azure Data Lake Storage Gen2 索引器（预览版）](search-howto-index-azure-data-lake-storage.md) | 为 Data Lake Storage Gen2 中的内容和元数据编制索引。| 
| [moreLikeThis 查询参数（预览版）](search-more-like-this.md) | 查找与特定文档相关的文档。 早期预览版中已有此功能。 | 

## <a name="management-features"></a>管理功能

|||
|-|-|
| IP 访问限制 | 使用管理 REST API 的 [`api-version=2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview)，可以创建对允许访问哪些 IP 地址进行限制的服务。 |

## <a name="earlier-preview-features"></a>早期预览版功能

早期预览版中公布的功能如果还没有转变为正式版，则仍然处于公共预览版。 如果你正在调用使用较早预览版 API 版本的 API，可以继续使用该版本或切换到 `2019-05-06-Preview`，而不会对预期行为作出任何更改。

## <a name="how-to-call-a-preview-api"></a>如何调用预览版 API

早期预览版仍然可用，但随着时间推移会变得过时。 如果代码调用 `api-version=2016-09-01-Preview` 或 `api-version=2017-11-11-Preview`，那些调用仍然有效。 但是，只有最新预览版会获得改进。 

以下示例语法说明了对预览 API 版本的调用。

    GET https://[service name].search.chinacloudapi.cn/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure 认知搜索服务在多个版本内可用。 有关详细信息，请参阅 [API 版本](search-api-versions.md)。

## <a name="next-steps"></a>后续步骤

查看搜索 REST API 参考文档。 如果遇到问题，请通过 [StackOverflow](https://stackoverflow.com/) 向我们寻求帮助，或[联系支持人员](https://support.azure.cn/support/contact)。

> [!div class="nextstepaction"]
> [搜索服务 REST API 参考](https://docs.microsoft.com/rest/api/searchservice/)