---
title: 新功能公告
titleSuffix: Azure Cognitive Search
description: 公告新的和增强的功能，包括将 Azure 搜索这项服务重命名为 Azure 认知搜索。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.openlocfilehash: 3c64ebda45f5b337059d1d90fb5248e7d90bb0ff
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348551"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure 认知搜索中的新增功能

了解该服务中的新增功能。 将此页添加为书签，以便及时了解该服务。

<a name="new-service-name"></a>

## <a name="new-service-name-for-azure-search"></a>Azure 搜索的新服务名称

Azure 搜索现已重命名为 **Azure 认知搜索**，这样可以反映认知技能的扩展使用以及在核心操作中进行的 AI 处理。 虽然认知技能添加了新功能，但使用 AI 是完全可选的。 可以继续在没有 AI 的情况下使用 Azure 认知搜索，以便在你在云中创建并管理的索引中针对专用的、异源的、基于文本的内容构建丰富的全文搜索解决方案。 

API 版本、Nuget 包、命名空间和终结点未更改。 现有的搜索解决方案不受服务名称更改的影响。

## <a name="feature-announcements"></a>功能公告

2019 年 11 月 4 日 - Ignite 大会

<!-- + [Incremental indexing (preview)](cognitive-search-incremental-indexing-conceptual.md) allows you to pick and choose which steps to reprocess when making modifications to an enrichment pipeline. Incremental indexing is useful if you have image content that you previously analyzed. The output of costly analysis is stored and then used as a basis for additional indexing or enrichment. -->

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

<!-- + [Document Extraction (preview)](cognitive-search-skill-document-extraction.md) is a cognitive skill used during indexing that allows you to extract the contents of a file from within a skillset. Previously, document cracking only occurred prior to skillset execution. With the addition of this skill, you can also perform this operation within skillset execution. -->

<!-- + [Text Translation (preview)](cognitive-search-skill-text-translation.md) is a cognitive skill used during indexing that evaluates text and, for each record, returns the text translated to the specified target language. -->

<!-- + [Power BI templates](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) can jumpstart your visualizations and analysis of enriched content in a knowledge store in Power BI desktop. This template is designed for Azure table projections created through the [Import data wizard](knowledge-store-create-portal.md). -->

+ 索引器现在支持 [Azure Data Lake Storage Gen2（预览版）](search-howto-index-azure-data-lake-storage.md)、[Cosmos DB Gremlin API（预览版）](search-howto-index-cosmosdb.md)和 [Cosmos DB Cassandra API（预览版）](search-howto-index-cosmosdb.md)。 可以使用[此窗体](https://aka.ms/azure-cognitive-search/indexer-preview)登录。 获允加入预览计划以后，你会收到确认电子邮件。

2019 年 7 月 24 日

<!-- + Generally available in [Azure Government Cloud](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search). -->

## <a name="service-updates"></a>服务更新

Azure 认知搜索的[服务更新公告](https://azure.microsoft.com/updates/?product=search&status=all)可以在 Azure 网站上找到。