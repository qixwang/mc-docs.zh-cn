---
title: moreLikeThis（预览版）查询功能
titleSuffix: Azure Cognitive Search
description: 介绍了 moreLikeThis（预览版）功能，该功能可在 Azure 认知搜索 REST API 的预览版中找到。
manager: nitinme
author: brjohnstmsft
ms.author: v-tawe
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.openlocfilehash: 3d2e4a5ff0980627d429f786c89d2e2fe64961d7
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336509"
---
# <a name="morelikethis-preview-in-azure-cognitive-search"></a>Azure 认知搜索中的 moreLikeThis（预览版）

> [!IMPORTANT] 
> 此功能目前处于公开预览状态。 提供的预览版功能不附带服务级别协议，我们不建议将其用于生产工作负荷。
> [REST API 版本 2019-05-06-Preview](search-api-preview.md) 提供了此功能。 目前不支持门户或 .NET SDK。

`moreLikeThis=[key]` 是[搜索文档 API](https://docs.microsoft.com/rest/api/searchservice/search-documents) 中的查询参数，用于查找与文档键所指定文档类似的文档。 当使用 `moreLikeThis` 生成搜索请求时，使用从最匹配的给定文档中提取的搜索项来生成查询。 然后使用生成的查询来生成搜索请求。 默认情况下，会考虑所有可搜索字段的内容，使用 `searchFields` 参数指定的限制字段除外。 `moreLikeThis` 参数不能与搜索参数 `search=[string]` 一起使用。

默认情况下，会考虑所有顶级的可搜索字段的内容。 若要改为指定特定字段，则可使用 `searchFields` 参数。 

不能在可以搜索的[复杂类型](search-howto-complex-data-types.md)的子字段上使用 moreLikeThis。

## <a name="examples"></a>示例 

下面是 moreLikeThis 查询的一个示例。 该查询会查找其描述字段与 `moreLikeThis` 参数指定的源文档字段最相似的文档。

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2019-05-06-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```


## <a name="next-steps"></a>后续步骤

可以使用任何 Web 测试工具来试验此功能。  建议使用 Postman 完成此练习。

> [!div class="nextstepaction"]
> [使用 Postman 探索 Azure 认知搜索 REST API](search-get-started-postman.md)