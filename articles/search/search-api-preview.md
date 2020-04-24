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
ms.date: 03/16/2020
ms.openlocfilehash: ed34f60e844b342174a3dc324e2af747a9efdda2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80243714"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Azure 认知搜索中的预览版功能

本文列出了预览版中当前提供的功能。 此列表中未包括已从预览版转变为正式版的功能。 有关正式版的公告，可以查看[服务更新](https://updates.azure.cn)或[新增功能](whats-new.md)。

虽然某些预览版功能在门户和 .NET SDK 中可能可用，但 REST API 始终具有预览版功能。

+ 对于搜索操作，当前预览版版本为 [ **`2019-05-06-Preview`** ](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview)。
+ 对于管理操作，当前预览版版本为 [ **`2019-10-01-Preview`** ](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview)。

<!-- > [!IMPORTANT] -->
<!-- > Preview functionality is provided without a service level agreement, and is not recommended for production workloads.-->

## <a name="ai-enrichment-features"></a>AI 扩充功能

通过[预览版搜索 API](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview) 探索 AI 扩充的最新增强功能。

|||
|-|-|
| [自定义实体查找技能（预览版）](cognitive-search-skill-custom-entity-lookup.md ) | 可在用户自定义的单词和短语列表中查找文本的一项认知技能。 它使用此列表为包含任何匹配实体的所有文档加上标签。 该技能还支持一定程度的模糊匹配，应用此匹配方法可以查找类似但不完全相同的匹配项。 | 
| [PII 检测技能（预览版）](cognitive-search-skill-pii-detection.md) | 编制索引期间使用的一项认知技能，它可以从输入文本中提取个人身份信息，并可让你通过多种方式在该文本中屏蔽此类信息。| 
| [增量扩充（预览版）](cognitive-search-incremental-indexing-conceptual.md) | 将缓存添加到扩充管道。如果有针对性的修改（例如更新技能集或另一对象）不会更改内容，则此功能可让你重复使用现有的输出。 缓存仅适用于技能集生成的扩充文档。| 
| [知识存储（预览版）](knowledge-store-concept-intro.md) | 基于 AI 的扩充管道的新目标。 物理数据结构存在于 Azure Blob 存储和 Azure 表存储中，它是在运行附有认知技能集的索引器时创建和填充的。 知识存储本身的定义在技能集定义中指定。 在知识存储定义中，可以通过投影元素控制数据的物理结构，这些元素确定如何塑造数据、是将数据存储在表存储中还是 Blob 存储中，以及是否存在多个视图。 | 

## <a name="indexing-and-query-features"></a>索引编制和查询功能

预览版搜索 API 中提供了索引器预览版功能。 

|||
|-|-|
| [Cosmos DB 索引器](search-howto-index-cosmosdb.md) | 支持 MongoDB API（预览版）、Gremlin API（预览版）和 Cassandra API（预览版）API 类型。 | 
|  [Azure Data Lake Storage Gen2 索引器（预览版）](search-howto-index-azure-data-lake-storage.md) | 为 Data Lake Storage Gen2 中的内容和元数据编制索引。| 
| [moreLikeThis 查询参数（预览版）](search-more-like-this.md) | 查找与特定文档相关的文档。 早期预览版中已有此功能。 | 

## <a name="management-features"></a>管理功能

<!-- | [Private Endpoint support](service-create-private-endpoint.md) | You can create a virtual network with a secure client (such as a virtual machine), and then create a search service that uses Private Endpoint. | -->

|||
|-|-|
| IP 访问限制 | 使用管理 REST API 的 [`api-version=2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview)，可以创建对允许访问哪些 IP 地址进行限制的服务。 |

## <a name="earlier-preview-features"></a>早期预览功能

早期预览版中公布的功能如果还没有转变为正式版，则仍然处于公共预览版。 如果你正在调用使用较早预览版 API 版本的 API，可以继续使用该版本或切换到 `2019-05-06-Preview`，而不会对预期行为作出任何更改。

## <a name="how-to-call-a-preview-api"></a>如何调用预览版 API

早期预览版仍然可用，但随着时间推移会变得过时。 如果代码调用 `api-version=2016-09-01-Preview` 或 `api-version=2017-11-11-Preview`，那些调用仍然有效。 但是，只有最新预览版会获得改进。 

以下示例语法说明了对预览 API 版本的调用。

    GET https://[service name].search.azure.cn/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure 认知搜索服务在多个版本内可用。 有关详细信息，请参阅 [API 版本](search-api-versions.md)。

## <a name="next-steps"></a>后续步骤

查看搜索 REST API 参考文档。 如果遇到问题，请通过 [StackOverflow](https://stackoverflow.com/) 向我们寻求帮助，或[联系支持人员](https://support.azure.cn/support/contact)。

> [!div class="nextstepaction"]
> [搜索服务 REST API 参考](https://docs.microsoft.com/rest/api/searchservice/)