---
title: 搜索 CSV Blob
titleSuffix: Azure Cognitive Search
description: 使用 delimitedText 分析模式（目前为公共预览版）通过 Azure Blob 存储提取和导入 CSV。
manager: nitinme
author: mgottein
ms.author: v-tawe
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.openlocfilehash: 8a03ecddde6263ff21f46d58f6c64264372b7d31
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336531"
---
# <a name="how-to-index-csv-blobs-using-delimitedtext-parsing-mode-and-blob-indexers-in-azure-cognitive-search"></a>如何使用 Azure 认知搜索中的 delimitedText 分析模式和 Blob 索引器为 CSV blob 编制索引 

> [!IMPORTANT] 
> delimitedText 分析模式当前为公共预览版。 提供的预览版功能不附带服务级别协议，我们不建议将其用于生产工作负荷。
> [REST API 版本 2019-05-06-Preview](search-api-preview.md) 提供了此功能。 目前不支持门户或 .NET SDK。

默认情况下，[Azure 认知搜索 Blob 索引器](search-howto-indexing-azure-blob-storage.md)会将分隔的文本 blob 分析为单个文本块。 但在 blob 含有 CSV 数据的情况下，通常希望将 blob 中的每一行视为一个单独文档。 例如，给定以下带分隔符的文本，可能要将其分析为两个文档，每个文档包含“id”、“datePublished”和“tags”字段： 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

本文介绍如何设置 `delimitedText` 分析模式，以便使用 Azure 认知搜索 Blob 索引器分析 CSV Blob。 

> [!NOTE]
> 请遵循[一对多索引](search-howto-index-one-to-many-blobs.md)中的索引器配置建议从一个 Azure Blob 输出多个搜索文档。

## <a name="setting-up-csv-indexing"></a>设置 CSV 索引
若要对 CSV blob 编制索引，请使用 `delimitedText` 分析模式根据[创建索引器](https://docs.microsoft.com/rest/api/searchservice/create-indexer)请求创建或更新索引器定义：

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

`firstLineContainsHeaders` 指示每个 blob 的第一行（非空）包含标头。
如果 blob 未包含初始标头行，则应在索引器配置中指定标头： 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

可以使用 `delimitedTextDelimiter` 配置设置来自定义分隔符字符。 例如：

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> 目前，仅支持 UTF-8 编码。

> [!IMPORTANT]
> 当使用分隔文本分析模式时，Azure 认知搜索假定数据源中的所有 blob 都将是 CSV。
> 
> 

## <a name="request-examples"></a>请求示例
汇总后，以下是完整的有效负载示例。 

数据源： 

    POST https://[service name].search.chinacloudapi.cn/datasources?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

索引器：

    POST https://[service name].search.chinacloudapi.cn/indexers?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

<!-- ## Help us make Azure Cognitive Search better -->
<!-- If you have feature requests or ideas for improvements, provide your input on [UserVoice](https://feedback.azure.com/forums/263029-azure-search/). -->

