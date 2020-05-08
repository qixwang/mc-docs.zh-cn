---
title: 简化的查询语法
titleSuffix: Azure Cognitive Search
description: Azure 认知搜索中用于全文搜索查询的简单查询语法的参考文档。
manager: nitinme
author: brjohnstmsft
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 04/12/2020
ms.date: 04/20/2020
ms.openlocfilehash: 6f64edcddc1b55e88b96c5a3371438fe0e19cad7
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588718"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Azure 认知搜索中的简单查询语法

Azure 认知搜索实现两种基于 Lucene 的查询语言：[简单查询分析器](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html)和 [Lucene 查询分析器](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)。 

在 Azure 认知搜索中，简单查询语法排除了模糊搜索操作。 请改用完整的 Lucene 语法进行[模糊搜索](search-query-fuzzy.md)。

> [!NOTE]
> 简单查询语法用于在[搜索文档](https://docs.microsoft.com/rest/api/searchservice/search-documents) API 的**搜索**参数中传递的查询表达式，不要与用于该 API 的 [$filter](search-filters.md) 参数的 [OData 语法](query-odata-filter-orderby-syntax.md)相混淆。 这两个不同的语法有各自的用于构造查询、转义字符串等操作的规则。
>
> Azure 认知搜索提供了替代的[完整 Lucene 查询语法](query-lucene-syntax.md)，用于在**搜索**参数中输入更复杂查询。 若要详细了解查询分析体系结构和每种语法的好处，请参阅 [Azure 认知搜索中全文搜索的工作原理](search-lucene-query-architecture.md)。

## <a name="invoke-simple-parsing"></a>调用简单分析

简单语法为默认语法。 仅当将语法从“完整”重置为“简单”时才需要调用。 若要显式设置语法，请使用 `queryType` 搜索参数。 有效值包括 `queryType=simple` 或 `queryType=full`（其中 `simple` 为默认值），`full` 调用[完整的 Lucene 查询分析程序](query-lucene-syntax.md)进行更高级的查询。 

## <a name="syntax-fundamentals"></a>语法基础知识

包含一个或多个词条的任何文本都被视为查询执行的有效起点。 Azure 认知搜索将匹配包含任何或所有词条的文档，其中包括在分析文本期间发现的任何变体。

尽管听起来很简单，但 Azure 认知搜索中的查询执行的一个方面*可能会*产生意外结果，导致搜索结果增加而不是减少，因为更多的词条和运算符被添加到输入字符串中。 这种扩展是否会实际发生取决于是否包含 NOT 运算符，以及组合使用的 searchMode  参数设置，该参数设置决定了如何根据 AND 或 OR 行为解释 NOT。 有关详细信息，请参阅 [NOT 运算符](#not-operator)。

### <a name="precedence-operators-grouping"></a>优先运算符（分组）

可以使用圆括号创建子查询，其包括附加说明语句中的运算符。 例如，`motel+(wifi||luxury)` 将搜索包含“motel”术语以及“wifi”或“luxury”（或两者）的文档。

字段分组与之类似，但将分组范围限定为单个字段。 例如，`hotelAmenities:(gym+(wifi||pool))` 在“hotelAmenities”字段中搜索“gym”和“wifi”，或者“gym”和“pool”。  

### <a name="escaping-search-operators"></a>转义搜索运算符  

若要使用任何搜索运算符作为搜索文本的一部分，请使用一个反斜杠 (`\`) 作为前缀对该字符进行转义。 例如，若要执行对 `https://` 的通配符搜索（其中 `://` 是查询字符串的一部分），需要指定 `search=https\:\/\/*`。 同样，转义的电话号码模式可能类似于 `\+1 \(800\) 642\-7676`。

需要转义的特殊字符包括下列项：`- * ? \ /`  

为了让更典型的情况变得简单，此规则有两个不需要进行转义的例外：  

+ 仅当 NOT 运算符 `-` 是空格之后的第一个字符时才需要对其进行转义，如果它位于词条中间则不需要对其进行转义。 例如，以下 GUID 在没有转义字符的情况下有效：`3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`。

+ 仅当后缀运算符 `*` 是空格之前的最后一个字符时才需要对其进行转义，如果它位于词条中间则不需要对其进行转义。 例如，`4*4=16` 不需要反斜杠。

> [!NOTE]  
> 尽管转义将标记保留在一起，但在编制索引期间，[词法分析](search-lucene-query-architecture.md#stage-2-lexical-analysis)可能会将它们去除。例如，标准 Lucene 分析器会删除连字符、空格和其他字符并在这些字符处拆分单词。 如果需要在查询字符串中使用特殊字符，则可能需要使用会将它们保留在索引中的分析器。 可供选择的一些项包括 Microsoft 自然[语言分析器](index-add-language-analyzers.md)（它会保留带连字符的单词）和自定义分析器（用于更复杂的模式）。 有关详细信息，请参阅[部分词语、模式和特殊字符](search-query-partial-matching.md)。

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>对 URL 中的不安全及保留字符进行编码

请确保对 URL 中的所有不安全和保留字符进行编码。 例如，“#”是不安全字符，因为它是 URL 中的片段/定位标识符。 如果用于 URL，则该字符必须编码为 `%23`。 由于“&”和“=”在 Azure 认知搜索中分隔参数并指定值，因而是保留字符的示例。 请参阅 [RFC1738：统一资源定位器 (URL)](https://www.ietf.org/rfc/rfc1738.txt) 获取更多详细信息。

不安全字符为 ``" ` < > # % { } | \ ^ ~ [ ]``。 保留字符为 `; / ? : @ = + &`。

###  <a name="query-size-limits"></a><a name="bkmk_querysizelimits"></a> 查询大小限制

 存在对可以发送到 Azure 认知搜索的查询大小的限制。 具体而言，最多可以有 1024 条子句（以 AND、OR 等分隔的表达式）。 此外，查询中任何单个术语的大小限制为大约 32 KB。 如果应用程序以编程方式生成搜索查询，则建议将其设计为不会生成无限大小的查询。  

## <a name="boolean-search"></a>布尔值搜索

可以在查询字符串中嵌入布尔运算符（AND、OR、NOT），以生成丰富的一组用于查找匹配文档的标准。 

### <a name="and-operator-"></a>AND 运算符 `+`

AND 运算符是一个加号。 例如，`wifi+luxury` 将搜索包含 `wifi` 和 `luxury` 的文档。

### <a name="or-operator-"></a>OR 运算符 `|`

OR 运算符是一个竖条或管状字符。 例如，`wifi | luxury` 将搜索包含 `wifi` 或 `luxury` 或两者的文档。

<a name="not-operator"></a>

### <a name="not-operator--"></a>NOT 运算符 `-`

NOT 运算符是一个减号。 例如：`wifi –luxury` 将搜索包含 `wifi` 词语且/或不包含 `luxury` 的文档。

查询请求中的 searchMode  参数控制具有 NOT 运算符的词语是通过 AND 运算符还是通过 OR 运算符与查询中的其他词语组合到一起（假定其他词语中没有 `+` 或 `|` 运算符）。 有效值包括 `any` 或 `all`。

`searchMode=any` 通过包含更多结果来提高查询的查全率，且默认情况下 `-` 会被解释为“OR NOT”。 例如，`wifi -luxury` 将匹配包含 `wifi` 词条或不包含 `luxury` 词条的文档。

`searchMode=all` 通过包含更少结果来提高查询的查准率，且默认情况下“-”会被解释为“AND NOT”。 例如，`wifi -luxury` 将匹配包含 `wifi` 词条且不包含“luxury”词条的文档。 这对于 `-` 运算符来说可能是更直观的行为。 因此，如果想要优化搜索的查准率（而非查全率），且  用户在搜索中频繁使用 `-` 运算符，则应考虑使用 `searchMode=all` 而不是 `searchMode=any`。

在决定 searchMode  设置时，请考虑不同应用程序中的查询的用户交互模式。 搜索信息的用户更有可能在查询中包含运算符，相对而言，电子商务网站具有更多的内置导航结构。

<a name="prefix-search"></a>

## <a name="prefix-search"></a>前缀搜索

后缀运算符是一个星号 `*`。 例如，`lingui*` 会查找“linguistic”或“linguini”并忽略大小写。 

与筛选器类似，前缀查询查找完全匹配项。 因此，不存在相关性评分（所有结果的搜索分数均为 1.0）。 前缀查询可能会很慢，尤其是在索引较大但前缀包含的字符数量较少的情况下。 

如果要执行后缀查询（针对字符串的最后一部分进行匹配），请使用[通配符搜索](query-lucene-syntax.md#bkmk_wildcard)和完整的 Lucene 语法。

## <a name="phrase-search-"></a>短语搜索 `"`

词语搜索是针对一个或多个词语的查询，其中任何词语都被视为一个匹配项。 短语搜索是用引号 `" "` 引起来的精确短语。 例如，`Roach Motel`（没有引号）会以任何顺序在任何位置搜索包含 `Roach` 和/或 `Motel` 的文档，而 `"Roach Motel"`（带引号）只会匹配包含整个短语并按该顺序排列的文档（文本分析仍然适用）。

## <a name="see-also"></a>另请参阅  

+ [简单搜索的查询示例](search-query-simple-examples.md)
+ [完整 Lucene 搜索的查询示例](search-query-lucene-examples.md)
+ [搜索文档（Azure 认知搜索 REST API）](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [Lucene 查询语法](query-lucene-syntax.md)
+ [OData 表达式语法](query-odata-filter-orderby-syntax.md) 
