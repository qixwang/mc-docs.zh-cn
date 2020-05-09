---
title: 创建建议器
titleSuffix: Azure Cognitive Search
description: 通过创建建议器并构建调用自动完成或自动建议的查询字词的请求，在 Azure 认知搜索中启用自动提示查询操作。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 04/14/2020
ms.date: 04/20/2020
ms.openlocfilehash: 16241084023d4ac69013f457f3b40502b0d9f6e1
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588794"
---
# <a name="create-a-suggester-to-enable-autocomplete-and-suggested-results-in-a-query"></a>创建建议器，以在查询中启用“自动完成”和“建议结果”功能

在 Azure 认知搜索中，“键入时搜索”是通过添加到[搜索索引](search-what-is-an-index.md)的建议器构造启用的  。 建议器支持两种体验：自动完成 - 完成字词或短语；建议 - 返回匹配文档的简短列表。    

取自[在 C# 中创建第一个应用](tutorial-csharp-type-ahead-and-suggestions.md)的以下屏幕截图演示了这两种体验。 自动完成可预测潜在字词，并使用“in”来补充“tw”。 建议是极其精简的搜索结果，其中的字段（例如酒店名称）表示索引中匹配的酒店搜索文档。 对于建议，可以呈现任何提供描述性信息的字段。

![自动完成和建议查询的直观比较](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "自动完成和建议查询的直观比较")

可以单独使用这些功能，或将它们一起使用。 有一个索引和查询组件可在 Azure 认知搜索中实现这些行为。 

+ 在索引中，将建议器添加到索引。 可以使用门户、[REST API](https://docs.microsoft.com/rest/api/searchservice/create-index) 或 [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)。 本文的余下内容重点介绍如何创建建议器。

+ 在查询请求中，调用[下面列出的 API](#how-to-use-a-suggester) 之一。

对于字符串字段，可按字段启用“键入时搜索”支持。 若要获得屏幕截图中所示的类似体验，可以在同一搜索解决方案中实现这两种自动提示行为。 这两个请求针对特定索引的文档集合，在用户提供至少包含三个字符的输入字符串后，将返回响应。 

## <a name="what-is-a-suggester"></a>什么是建议器？

建议器是一种数据结构，它通过存储用于匹配部分查询的前缀来支持“键入时搜索”行为。 类似于标记化字词，前缀存储在倒排索引中，建议器字段集合中指定的每个字段都有一个倒排索引。

创建前缀时，建议器具有自己的分析链，类似于用于全文搜索的分析链。 但是，与全文搜索中的分析不同，建议器只能对使用标准 Lucene 分析器（默认）或[语言分析器](index-add-language-analyzers.md)的字段运行。 使用[自定义分析器](index-add-custom-analyzers.md)或[预定义分析器](index-add-custom-analyzers.md#predefined-analyzers-reference)（标准 Lucene 除外）的字段被明确禁止，这样是为了防止结果不佳。

> [!NOTE]
> 如果需要绕开分析器约束，请对相同的内容使用两个不同的字段。 这样，就可以在其中一个字段中使用建议器，并使用自定义分析器配置来设置其他字段。

## <a name="define-a-suggester"></a>定义建议器

若要创建建议器，请将一个建议器添加到[索引架构](https://docs.microsoft.com/rest/api/searchservice/create-index)并[设置每个属性](#property-reference)。 在索引中只能包含一个建议器（具体说来，建议器集合中只能有一个建议器）。 创建建议器的最佳时间是还要定义使用建议器的字段时。

### <a name="choose-fields"></a>选择字段

虽然建议器有多个属性，但它主要是需要为其启用“键入时搜索”体验的字段集合。 具体对于建议而言，请选择最能代表单个结果的字段。 用于区分多个匹配项的名称、标题或其他唯一字段最合适。 如果字段包含重复值，则建议会包含相同的结果，因此用户不知道要单击哪条建议。

确保每个字段使用在编制索引期间执行词法分析的分析器。 可以使用默认的标准 Lucene 分析器 (`"analyzer": null`) 或[语言分析器](index-add-language-analyzers.md)（例如 `"analyzer": "en.Microsoft"`）。 

所选的分析器决定了如何标记化字段并随后指定其前缀。 例如，对于带连字符的字符串（例如“context-sensitive”），使用语言分析器会生成以下标记组合：“context”、“sensitive”、“context-sensitive”。 如果使用的是标准 Lucene 分析器，则带连字符的字符串不存在。

> [!TIP]
> 考虑使用[分析文本 API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) 来深入了解如何标记化字词并随后指定其前缀。 生成索引后，可以尝试对字符串运行各种分析器，以查看分析器发出的标记。

### <a name="when-to-create-a-suggester"></a>何时创建建议器

创建建议器的最佳时间是同时要创建字段定义本身时。

如果尝试使用预先存在的字段创建建议器，API 将不允许这样做。 在编制索引期间，如果两个或更多个字符的组合中的部分字词连同完整字词一起标记化，则会生成前缀。 如果现有字段已标记化，而你想要将其添加到建议器，则必须重新生成索引。 有关详细信息，请参阅[如何重新生成 Azure 认知搜索索引](search-howto-reindex.md)。

## <a name="create-using-rest"></a>使用 REST 进行创建

在 REST API 中，通过[创建索引](https://docs.microsoft.com/rest/api/searchservice/create-index)或[更新索引](https://docs.microsoft.com/rest/api/searchservice/update-index)添加建议器。 

  ```json
  {
    "name": "hotels-sample-index",
    "fields": [
      . . .
          {
              "name": "HotelName",
              "type": "Edm.String",
              "facetable": false,
              "filterable": false,
              "key": false,
              "retrievable": true,
              "searchable": true,
              "sortable": false,
              "analyzer": "en.microsoft",
              "indexAnalyzer": null,
              "searchAnalyzer": null,
              "synonymMaps": [],
              "fields": []
          },
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["HotelName"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```

## <a name="create-using-net"></a>使用 .NET 进行创建

在 C# 中定义[建议器对象](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)。 `Suggesters` 是一个集合，但它只能采用一个项。 

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels-sample-index",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelName", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

## <a name="property-reference"></a>属性参考

|属性      |说明      |
|--------------|-----------------|
|`name`        |建议器的名称。|
|`searchMode`  |用于搜索候选短语的策略。 目前支持的唯一模式是 `analyzingInfixMatching`，该模式目前匹配字词的开头。|
|`sourceFields`|作为建议内容源的一个或多个字段的列表。 字段的类型必须是 `Edm.String` 和 `Collection(Edm.String)`。 如果在字段中指定某个分析器，该分析器必须是[此列表](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername?view=azure-dotnet)中指定的分析器（而不是自定义分析器）。<p/> 作为最佳做法，请仅指定有助于生成预期相应响应的字段，无论该响应是搜索栏还是下拉列表中的已完成字符串。<p/>酒店名称就是一很好的候选项，因为它很精确。 说明和注释等详细字段过于密集。 同样，类别和标记等重复性字段的效率较低。 在示例中，我们仍然包含了“category”来演示可以包含多个字段。 |

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester"></a>使用建议器

在查询中使用建议器。 创建建议器后，请调用以下 API 之一来实现“键入时搜索”体验：

+ [建议 REST API](https://docs.microsoft.com/rest/api/searchservice/suggestions) 
+ [自动完成 REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 
+ [SuggestWithHttpMessagesAsync 方法](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [AutocompleteWithHttpMessagesAsync 方法](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

在搜索应用程序中，客户端代码应利用 [jQuery UI Autocomplete](https://jqueryui.com/autocomplete/) 之类的库来收集部分查询并提供匹配项。 有关此任务的详细信息，请参阅[将“自动完成”或“建议结果”功能添加到客户端代码](search-autocomplete-tutorial.md)。

以下对自动完成 REST API 的调用演示了 API 的用法。 此示例有两个要点。 首先，与所有查询一样，操作是针对索引的文档集合执行的，查询包含一个 search 参数，在本例中该参数提供部分查询  。 其次，必须将 suggesterName 添加到请求  。 如果未在索引中定义建议器，对自动完成或建议的调用将会失败。

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2019-05-06
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

## <a name="sample-code"></a>代码示例

+ [在 C# 中创建第一个应用（第 3 课 - 添加“键入时搜索”）](tutorial-csharp-type-ahead-and-suggestions.md)示例演示了建议器的构造、建议的查询、自动完成和分面导航。 此代码示例在沙盒 Azure 认知搜索服务中运行，并使用预先加载的酒店索引，因此，只需按 F5 即可运行应用程序。 无需订阅或登录。

+ [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) 是包含 C# 和 Java 代码的早期示例。 其中也演示了建议器的构造、建议的查询、自动完成和分面导航。 此代码示例使用托管的 [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) 示例数据。 

## <a name="next-steps"></a>后续步骤

建议查看以下示例来了解如何构建请求。

> [!div class="nextstepaction"]
> [向客户端代码添加“自动完成”和“建议”功能](search-autocomplete-tutorial.md) 
