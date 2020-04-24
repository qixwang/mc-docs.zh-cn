---
title: 匹配模式和特殊字符
titleSuffix: Azure Cognitive Search
description: 使用通配符和前缀查询来匹配 Azure 认知搜索查询请求中的完整或部分字词。 可以使用完整查询语法和自定义分析器来解析包含特殊字符的难以匹配的模式。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 01/14/2020
ms.date: 03/02/2020
ms.openlocfilehash: 4b096e7464d19d5f814979331d9cb0a85061a26b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77504176"
---
# <a name="match-on-patterns-and-special-characters-dashes"></a>模式和特殊字符（短划线）匹配

对于包含特殊字符 (`-, *, (, ), /, \, =`) 的查询或者基于较大字词中部分字词的查询模式，通常需要执行额外的配置步骤，确保索引包含适当格式的预期内容。 

默认情况下，类似于 `+1 (425) 703-6214` 的电话号码将标记化为 `"1"`、`"425"`、`"703"`、`"6214"`。 可以想象到，对 `"3-62"`（包含短划线的部分字词）进行搜索将会失败，因为该内容实际上在索引中并不存在。 

如果需要对部分字符串或特殊字符进行搜索，可以使用自定义分析器替代默认分析器，该自定义分析器按更简单的标记规则运行，当查询字符串包含字词部分或特殊字符时，它可以保留必要的完整字词。 退一步讲，该方法如下所述：

+ 选择预定义的分析器，或定义一个可生成所需输出的自定义分析器
+ 将分析器分配到字段
+ 生成索引并测试

本文将引导你完成这些步骤。 此处所述的方法在其他方案中也很有用：通配符和正则表达式查询也需要将完整字词用作模式匹配的基础。 

> [!TIP]
> 评估分析器是需要频繁重建索引的迭代过程。 可以使用 Postman 以及[创建索引](https://docs.microsoft.com/rest/api/searchservice/create-index)、[删除索引](https://docs.microsoft.com/rest/api/searchservice/delete-index)、[加载文档](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)和[搜索文档](https://docs.microsoft.com/rest/api/searchservice/search-documents) REST API 来简化此步骤。 使用“加载文档”时，请求正文应包含要测试的小型代表性数据集（例如，包含电话号码或产品代码的字段）。 在同一 Postman 集合中使用这些 API 可以快速循环执行这些步骤。

## <a name="choosing-an-analyzer"></a>选择分析器

选择可生成完整字词标记的分析器时，以下分析器是常用选项：

| 分析器 | 行为 |
|----------|-----------|
| [keyword](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) | 整个字段的内容将标记化为单个字词。 |
| [whitespace](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html) | 仅在空白位置分隔。 包含短划线或其他字符的字词被视为单个标记。 |
| [自定义分析器](index-add-custom-analyzers.md) | （建议）创建自定义分析器可以指定标记器 (tokenizer) 和标记筛选器。 以前的分析器必须按原样使用。 使用自定义分析器可以选取要使用的标记器和标记筛选器。 <br><br>建议的组合是[关键字标记器](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html)加[小写标记筛选器](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html)。 预定义的[关键字分析器](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html)本身不会将任何大写文本转换为小写，这可能导致查询失败。 自定义分析器提供一个机制用于添加小写标记筛选器。 |

如果使用 Postman 等 Web API 测试工具，则可以添加[测试分析器 REST 调用](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)来检查标记化的输出。 对于现有的索引以及包含短划线或部分字词的字段，可以针对特定字词尝试各种分析器，以查看发出哪些标记。  

1. 检查标准分析器，以查看默认情况下如何标记化字词。

   ```json
   {
   "text": "SVP10-NOR-00",
   "analyzer": "standard"
   }
    ```

1. 评估响应，以查看文本如何在索引中标记化。 注意每个字词如何转换为小写和分解。

    ```json
    {
        "tokens": [
            {
                "token": "svp10",
                "startOffset": 0,
                "endOffset": 5,
                "position": 0
            },
            {
                "token": "nor",
                "startOffset": 6,
                "endOffset": 9,
                "position": 1
            },
            {
                "token": "00",
                "startOffset": 10,
                "endOffset": 12,
                "position": 2
            }
        ]
    }
    ```
1. 修改请求以使用 `whitespace` 或 `keyword` 分析器：

    ```json
    {
    "text": "SVP10-NOR-00",
    "analyzer": "keyword"
    }
    ```

1. 现在，响应包含单个大写的标记，其中包含作为字符串一部分保留的短划线。 如果需要对模式或部分字词进行搜索，查询引擎现在可以用作查找匹配项的基础。


    ```json
    {

        "tokens": [
            {
                "token": "SVP10-NOR-00",
                "startOffset": 0,
                "endOffset": 12,
                "position": 0
            }
        ]
    }
    ```
> [!Important]
> 请注意，在生成查询树时，查询分析程序往往会将搜索表达式中的字词小写。 如果使用的分析器不会将文本输入小写，并且你未获得预期的结果，则原因可能就在于此。 解决方法是添加小写标记筛选器。

## <a name="analyzer-definitions"></a>分析器定义
 
无论你是在评估分析器还是继续进行特定的配置，都需要在字段定义中指定分析器，如果未使用内置分析器，则可能还需要配置分析器本身。 交换分析器时，通常需要重建索引（删除、重新创建并重新加载）。 

### <a name="use-built-in-analyzers"></a>使用内置分析器

可以在字段定义的 `analyzer` 属性中按名称指定内置或预定义的分析器，不需要在索引中进行其他配置。 以下示例演示如何在字段中设置 `whitespace` 分析器。

```json
    {
      "name": "phoneNumber",
      "type": "Edm.String",
      "key": false,
      "retrievable": true,
      "searchable": true,
      "analyzer": "whitespace"
    }
```
有关所有可用内置分析器的详细信息，请参阅[预定义分析器列表](https://docs.azure.cn/search/index-add-custom-analyzers#predefined-analyzers-reference)。 

### <a name="use-custom-analyzers"></a>使用自定义分析器

如果使用[自定义分析器](index-add-custom-analyzers.md)，请在索引中使用用户定义的标记器、标记筛选器和可能配置设置的组合来定义该分析器。 接下来，在字段定义中引用它，就像引用内置分析器一样。

如果目标是完整字词标记化，我们建议使用一个由**关键字标记器**和**小写标记筛选器**组成的自定义分析器。

+ 关键字标记器为字段的整个内容创建单个标记。
+ 小写标记筛选器将大写字母转换为小写文本。 查询分析程序通常将任何大写文本输入小写。 小写过程可将包含标记化字词的输入均匀化。

以下示例演示了一个提供关键字标记器和小写标记筛选器的自定义分析器。

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
]

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": []
```

> [!NOTE]
> `keyword_v2` 标记器和 `lowercase` 令牌筛选器对于系统是已知的，它们使用默认配置，正因如此，可以按名称引用它们，而无需先定义它们。

## <a name="tips-and-best-practices"></a>提示和最佳实践

### <a name="tune-query-performance"></a>调整查询性能

如果实现包含 keyword_v2 标记器和小写标记筛选器的建议配置，可能会注意到查询性能下降，因为需要对索引中的现有标记进行额外的标记筛选处理。 

以下示例添加一个 [EdgeNGramTokenFilter](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenizer.html)，使前缀匹配速度更快。 为包含字符的 2-25 个字符的组合生成额外的标记：（不局限于 MS、MSF、MSFT、MSFT/、MSFT/S、MSFT/SQ、MSFT/SQL）。 可以想象到，额外的标记化操作会导致索引变得更大。

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
]

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase", "my_edgeNGram"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": [
  {
  "@odata.type":"#Microsoft.Azure.Search.EdgeNGramTokenFilterV2",
  "name":"my_edgeNGram",
  "minGram": 2,
  "maxGram": 25,
  "side": "front"
  }
]
```

### <a name="use-different-analyzers-for-indexing-and-query-processing"></a>使用不同的分析器进行索引编制和查询处理

在编制索引和执行查询期间调用分析器。 我们通常使用同一个分析器来完成这两项操作，但可为每个工作负荷配置自定义分析器。 在[索引定义](https://docs.microsoft.com/rest/api/searchservice/create-index)的 `analyzers` 节中指定分析器重写，然后在特定的字段中引用重写。 

如果只是在编制索引期间需要自定义分析，则可以应用自定义分析器来仅完成索引编制，并继续使用标准 Lucene 分析器（或其他分析器）完成查询。

若要指定角色特定的分析，可在每个角色的字段中设置属性：设置 `indexAnalyzer` 和 `searchAnalyzer`，而不是默认的 `analyzer` 属性。

```json
"name": "featureCode",
"indexAnalyzer":"my_customanalyzer",
"searchAnalyzer":"standard",
```

### <a name="duplicate-fields-for-different-scenarios"></a>不同方案的重复字段

另一种做法利用按字段的分析器分配，以根据不同的方案进行优化。 具体而言，可以定义“featureCode”和“featureCodeRegex”，以支持先执行普通的全文搜索，再执行高级模式匹配的操作。

```json
{
  "name": "featureCode",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": null
},
{
  "name": "featureCodeRegex",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": "my_customanalyzer"
},
```

## <a name="next-steps"></a>后续步骤

本文说明了分析器如何造成查询问题和解决查询问题。 接下来，请更详细地了解分析器对索引编制和查询处理的影响。 具体而言，请考虑使用分析文本 API 返回标记化的输出，以便可以确切地了解分析器正在为索引创建哪些内容。

+ [语言分析器](search-language-support.md)
+ [Azure 认知搜索中用于文本处理的分析器](search-analyzers.md)
+ [分析文本 API (REST)](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)
+ [全文搜索的工作原理（查询体系结构）](search-lucene-query-architecture.md)