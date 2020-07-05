---
title: 新增功能通告
titleSuffix: Azure Cognitive Search
description: 新增功能和增强功能的通告，包括 Azure 搜索服务已重命名为 Azure 认知搜索。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: overview
origin.date: 06/08/2020
ms.date: 07/02/2020
ms.openlocfilehash: 6c83d99b7fca9f2d84fda995dbf8ac5e2745ba69
ms.sourcegitcommit: 5afd7c4c3be9b80c4c67ec55f66fcf347aad74c6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85942540"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure 认知搜索中的新增功能

了解服务中的新增功能。 请将本页加入书签，以随时了解该服务的最新信息。

## <a name="feature-announcements"></a>功能增强

<!--
### June 2020

Azure Machine Learning skill is new skill type to integrate an inferencing endpoint from Azure Machine Learning. The portal experience supports discovery and integration of your Azure Machine Learning endpoint within a Cognitive Search skillset. The discovery requires your Cognitive Search and Azure ML services be deployed in the same subscription. To sign up for the AML skill preview, [please fill out the form](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0jK7x7HQYdDm__YfEsbtcZUMTFGTFVTOE5XMkVUMFlDVFBTTlYzSlpLTi4u). Get started with [this tutorial](cognitive-search-tutorial-aml-custom-skill.md).
-->

<!--
### May 2020 (Microsoft Build)

+ [Debug sessions](cognitive-search-debug-session.md) feature is now in preview. [Sign up to request access](https://aka.ms/DebugSessions). Debug sessions provides a portal-based interface to investigate and resolve issues with a skillset. Fixes created in the debug session can be saved to production skillsets. Get started with [this tutorial](cognitive-search-tutorial-debug-sessions.md).
+ Security enhancements include the ability to [set up a private search endpoint (preview)](service-create-private-endpoint.md) that is inaccessible on the public internet. You can also [configure IP rules for in-bound firewall support (preview)](service-configure-firewall.md).

+ Use a [system-managed identity (preview)](search-howto-managed-identities-data-sources.md) to set up a connection to an Azure data source for indexing. Applies to [indexers](search-indexer-overview.md) that ingest content from Azure data sources such as Azure SQL Database, Azure Cosmos DB, and Azure Storage.

+ Change the basis for how search scores are computed, from per-shard to all-shards, using the [scoringStatistics=global](index-similarity-and-scoring.md#scoring-statistics) and sessionId query parameters.
-->

### <a name="march-2020"></a>2020 年 3 月

+ [本机 blob 软删除（预览版）](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection)意味着 Azure 认知搜索中的 Azure blob 存储索引器会识别处于软删除状态的 blob，并在编制索引过程中删除相应的搜索文档。

+ 新的稳定版[管理 REST API (2020-03-13)](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) 现已可用。 

### <a name="february-2020"></a>2020 年 2 月

+ [PII 检测（预览版）](cognitive-search-skill-pii-detection.md)是编制索引期间使用的一项认知技能，它可以从输入文本中提取个人身份信息，并可让你通过多种方式在该文本中屏蔽此类信息。

+ [自定义实体查找（预览版）](cognitive-search-skill-custom-entity-lookup.md )可在用户自定义的单词和短语列表中查找文本。 它使用此列表为包含任何匹配实体的所有文档加上标签。 该技能还支持一定程度的模糊匹配，应用此匹配方法可以查找类似但不完全相同的匹配项。 

### <a name="january-2020"></a>2020 年 1 月

+ [客户管理的加密密钥](search-security-manage-encryption-keys.md)现已推出正式版。 如果使用 REST，可以通过 `api-version=2019-05-06` 访问该功能。 对于托管代码，即使该功能现在不再以预览版提供，但正确的包仍是 [.NET SDK 版本 8.0-preview](search-dotnet-sdk-migration-version-9.md)。 

+ 可以通过两种机制（两者的功能目前以预览版提供）对搜索服务进行私密访问：

  + 可以使用管理 REST API `api-version=2019-10-01-Preview` 创建服务，来仅限特定的 IP 地址进行访问。 预览版 API 在 [CreateOrUpdate API](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service) 中提供 **IpRule** 和 **NetworkRuleSet** 属性。 此预览版功能可在选定的区域中使用。 有关详细信息，请参阅[如何使用管理 REST API](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api)。

  <!-- + Currently available through a limited-access preview, you can provision an Azure Search service that supports Azure Private Endpoint for connections from clients on the same virtual network. For more information, see [Create a Private Endpoint for a secure connection](service-create-private-endpoint.md). -->

### <a name="december-2019"></a>2019 年 12 月

+ [创建应用（预览版）](search-create-app-portal.md)是门户中用于生成可下载 HTML 文件的新向导。 文件附带了嵌入式脚本，用于呈现可正常运行的“localhost”式 Web 应用，该应用绑定到搜索服务中的索引。 页面在向导中可配置，可以包含搜索栏、结果区域、边栏导航和自动提示查询支持。 可以脱机修改 HTML，以便扩展或自定义工作流或外观。

<!-- + [Create a private endpoint for secure connections (preview)](service-create-private-endpoint.md) explains how to set up a Private Link for secure connections to your search service. This preview feature is available upon request and uses [Azure Private Link](../private-link/private-link-overview.md) and [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) as part of the solution. -->

### <a name="november-2019---ignite-conference"></a>2019 年 11 月 - Ignite 大会

+ [增量扩充（预览版）](cognitive-search-incremental-indexing-conceptual.md)将缓存和有状态性添加到了扩充管道，使你能够处理特定的步骤或阶段，且不丢失已处理的内容。 以前，对扩充管道进行任何更改都需要重新生成整个管道。 使用增量扩充时，会保留高开销分析（尤其是图像分析）的输出。

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ [文档提取（预览版）](cognitive-search-skill-document-extraction.md)是编制索引期间使用的一项认知技能，用于从技能集中提取文件内容。 以前，文档破解只能在技能集执行之前发生。 使用此项新增技能，还可以在技能集的执行过程中执行此操作。

+ [文本翻译](cognitive-search-skill-text-translation.md)是编制索引期间使用的一项认知技能，它可以评估文本，并为每个记录返回翻译成指定目标语言的文本。

+ [Power BI 模板](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md)可以在 Power BI Desktop 中立即开始可视化和分析知识存储中的扩充内容。 此模板适用于通过[导入数据向导](knowledge-store-create-portal.md)创建的 Azure 表投影。

+ 索引器现在支持 [Azure Data Lake Storage Gen2（预览版）](search-howto-index-azure-data-lake-storage.md)、[Cosmos DB Gremlin API（预览版）](search-howto-index-cosmosdb.md)和 [Cosmos DB Cassandra API（预览版）](search-howto-index-cosmosdb.md)。 可以使用[此表单](https://aka.ms/azure-cognitive-search/indexer-preview)进行注册。 获允加入预览计划以后，你会收到确认电子邮件。

<!-- ### July 2019 -->

<!-- + Generally available in [Azure Government Cloud](https://docs.microsoft.com/azure/azure-government/documentation-government-services-webandmobile#azure-cognitive-search). -->

## <a name="service-updates"></a>服务更新

在 Azure 网站上可以找到 Azure 认知搜索的[服务更新通告](https://updates.azure.cn)。