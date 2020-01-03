---
title: 层和 SKU 的服务限制
titleSuffix: Azure Cognitive Search
description: 用于容量计划的服务限制以及请求和响应 Azure 认知搜索的最大限制。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.openlocfilehash: eabdc79db815c796280042ef6b7915a12a3f0084
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336517"
---
# <a name="service-limits-in-azure-cognitive-search"></a>Azure 认知搜索中的服务限制

对存储、工作负荷以及索引、文档和其他对象数量的最大限制，取决于是在“免费”、“基本”、“标准”还是“存储优化”定价层上[预配 Azure 认知搜索](search-create-service-portal.md)     。

+ **免费**层是 Azure 订阅随附的多租户共享服务。 索引和查询请求针对其他租户使用的副本和分区执行。

+ “基本”层为较小规模的生产工作负荷提供专用计算资源，但与其他租户共享某些网络基础结构。 

+ **标准**层在专用计算机上运行，在每个级别上都具有更多存储和处理容量。 标准层共有四个级别：S1、S2、S3 和 S3 HD。

+ **存储优化**层在专用计算机上运行，与**标准**层相比具有更多的总存储、存储带宽和内存。 “存储优化”层分为两个级别：L1 和 L2

> [!NOTE]
> 从 7 月 1 日起，所有层通常都可用，包括存储优化层。 所有定价都可以在[定价详细信息](https://www.azure.cn/pricing/details/search/)页上找到。

  S3 高密度 (S3 HD) 是针对特定工作负荷设计的：[多租户](search-modeling-multitenant-saas-applications.md)和大量的小索引（每个索引一百万个文档，每个服务三千个索引）。 此层未提供[索引器功能](search-indexer-overview.md)。 在 S3 HD 上，数据引入必须利用推送方式，使用 API 调用将数据从源推送到索引。 

> [!NOTE]
> 服务已在特定层上进行预配。 跳转层以获取容量涉及预配新的服务（不会就地升级）。 有关详细信息，请参阅[选择 SKU 或层](search-sku-tier.md)。 若要详细了解在已预配的服务内调整容量，请参阅[缩放查询和为工作负荷编制索引的资源级别](search-capacity-planning.md)。
>

## <a name="subscription-limits"></a>订阅限制
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>存储限制
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>索引限制

| 资源 | 免费 | 基本&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| 最大索引数 |3 |5 或 15 |50 |200 |200 |每个分区 1000，或者每个服务 3000 |10 个 |10 个 |
| 每个索引的简单字段的最大数目 |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| 每个索引的复杂集合字段的最大数目 |40 |40 |40 |40 |40 |40 |40 |40 |
| 每个文档的所有复杂集合的最大元素数目&nbsp;<sup>2</sup> |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| 复杂字段的最大深度 |10 个 |10 个 |10 个 |10 个 |10 个 |10 个 |10 个 |10 个 |
| 每个索引最大[建议器](https://docs.microsoft.com/rest/api/searchservice/suggesters)数 |1 |1 |1 |1 |1 |1 |1 |1 |
| 每个索引的最大[计分配置文件](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)数 |100 |100 |100 |100 |100 |100 |100 |100 |
| 每个配置文件的最大函数数量 |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> 在 2017 年 12 月之前创建的基本服务的索引数限制较低（为 5 而不是 15）。 基本层是唯一具有下限（每个索引 100 个字段）的 SKU。

<sup>2</sup> 目前，在每个文档的复杂集合中使用极大量的元素会导致较高的存储用量。 这是已知问题。 同时，限制 3000 是所有服务层级的安全上限。 此限制仅对利用支持复杂类型字段 (`2019-05-06`) 的最早正式版 (GA) API 的索引操作强制实施。 为了避免中断可能正在使用早期 API 预览版（支持复杂类型字段）的客户端，我们不会对使用这些 API 预览版的索引操作强制实施此限制。 请注意，API 预览版并不旨在用于生产方案，我们强烈建议客户迁移到最新的 API 正式版。

<a name="document-limits"></a>

## <a name="document-limits"></a>文档限制 

自 2018 年 10 月起，在任何区域的任何可计费层（基本、S1，S2、S3、S3 HD）创建的任何新服务都不再有任何文档数限制<sup>1</sup>。 虽然自 2017 年 11 月/12 月以来大多数区域的文档数量不受限制，但仍有五个区域继续实施文档数限制。 根据你创建搜索服务的时间和地点，你可能正在运行仍受文档数限制的服务。

若要确定你的服务是否具有文档数限制，请检查你的服务的概述页中的“使用情况”磁贴。 文档数可能不受限制，也可能基于层受限于某个限制。

  ![“使用情况”磁贴](media/search-limits-quotas-capacity/portal-usage-tile.png)

<sup>1</sup> 尽管没有任何 SKU 特定的文档限制，但每个索引仍需符合最大安全限制，以确保服务的稳定性。 此限制是 Lucene 实施的。 每个 Azure 认知搜索文档在内部作为一个或多个 Lucene 文档编制索引。 每个搜索文档的 Lucene 文档数取决于复杂集合字段中的元素总数。 每个元素作为一个单独的 Lucene 文档编制索引。 例如，在复杂集合字段中包含 3 个元素的文档将作为 4 个 Lucene 文档编制索引 - 1 个索引针对该文档本身，3 个索引针对元素。 每个索引的最大 Lucene 文档数大约为 250 亿。

### <a name="regions-previously-having-document-limits"></a>以前具有文档数限制的区域

如果门户指示文档数限制，则你的服务要么是在 2017 年底之前创建的，要么是在使用容量较低的群集托管 Azure 认知搜索服务的数据中心上创建的：

+ 澳大利亚东部
+ 东亚
+ 印度中部
+ 日本西部
+ 美国中西部

对于具有文档数限制的服务，将应用以下上限：

|  免费 | 基本 | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10,000 |1&nbsp;百万 |每个分区 1500 万，或者每个服务 1 亿 8 千万 |每个分区 6 千万，或者每个服务 7 亿 2 千万 |每个分区 1 亿 2 千万，或者每个服务 14 亿 |每个索引 1 百万，或者每个分区 2 亿 |

如果你的服务具有阻止你的限制，请创建一个新服务，然后将所有内容重新发布到该服务。 没有任何机制可以在后台无缝地将你的服务重新预配到新硬件上。

> [!Note] 
> 对于 2017 年底后创建的 S3 高密度服务，已去除每个分区 2 亿个文档的限制，但每个索引 100 万个文档的限制保留。


### <a name="document-size-limits-per-api-call"></a>每个 API 调用的文档大小限制

调用索引 API 时的最大文档大小大约为 16 MB。

文档大小实际上是索引 API 请求主体大小的限制。 由于可以将包含多个文档的批次传递给索引 API，因此大小限制实际上取决于批次中的文档数。 对于具有单个文档的批次，最大文档大小是 16 MB JSON。

要保持较低的文档大小，务必将不可查询数据从请求中排除。 图像和其他二进制数据不可直接查询，并且不应存储在索引中。 要将不可查询的数据集成到搜索结果中，请定义用于存储资源的 URL 引用的不可搜索字段。

## <a name="indexer-limits"></a>索引器限制

存在最长运行时间，目的是在总体上为服务提供平衡和稳定性，但较大的数据集需要的索引时间可能比最大值允许的要长。 如果在允许的最长时间内无法完成索引作业，请尝试按计划运行。 计划程序将跟踪索引的状态。 如果计划的索引作业因某种原因而中断，则索引器可以在下一次计划运行时从它上次停止的位置重新开始。


| 资源 | 免费&nbsp;<sup>1</sup> | 基本&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| 最大索引器数 |3 |5 或 15|50 |200 |200 |不适用 |10 个 |10 个 |
| 最大数据源数 |3 |5 或 15 |50 |200 |200 |不适用 |10 个 |10 个 |
| 每次调用的最大索引编制负载 |10,000 个文档 |仅受最大文档的限制 |仅受最大文档的限制 |仅受最大文档的限制 |仅受最大文档的限制 |不适用 |无限制 |无限制 |
| 最小计划 | 5 分钟 |5 分钟 |5 分钟 |5 分钟 |5 分钟 |5 分钟 |5 分钟 | 5 分钟 |
| 最长运行时间 <sup>5</sup> | 1-3 分钟 |24 小时 |24 小时 |24 小时 |24 小时 |不适用  |24 小时 |24 小时 |
| Blob 索引器：最大 blob 大小，MB |16 |16 |128 |256 |256 |不适用  |256 |256 |
| Blob 索引器：从 blob 中提取的内容的最大字符数 |32,000 |64,000 |4&nbsp;百万 |4&nbsp;百万 |4&nbsp;百万 |不适用 |4&nbsp;百万 |4&nbsp;百万 |

<sup>1</sup> 对于免费服务，对于 blob 源，索引器最长执行时间为 3 分钟；对于所有其他数据源，索引器最长执行时间为为 1 分钟。

<sup>2</sup> 在 2017 年 12 月之前创建的基本服务的索引器和数据源的数目限制较低（为 5 而不是 15）。

<sup>3</sup> S3 HD 服务未包括索引器支持。

<sup>4</sup> 认知搜索工作负荷和 Azure Blob 索引中的 图像分析的运行时间比常规文本索引运行时间短。 图像分析和自然语言处理属于计算密集型，并且消耗了过多的可用处理能力。 减少运行时间，以便队列中的其他作业能够运行。  

> [!NOTE]
> 如[索引限制](#index-limits)中所述，从支持复杂类型 (`2019-05-06`) 的最新 API 正式版开始，索引器还针对每个文档的所有复杂集合强制实施 3000 个元素的上限。 这意味着，如果你使用 API 旧版本创建了索引器，则不需要遵守此限制。 为了保持最高兼容性，使用 API 旧版本创建，然后使用 API 版本 `2019-05-06` 或更高版本更新的索引器，仍会从这些限制中**排除**。 客户应注意使用极大复杂集合所造成的负面影响（如前所述）；我们强烈建议使用最新 API 正式版创建任何新索引器。

## <a name="synonym-limits"></a>同义词限制

允许的同义词映射的最大数量因定价层而异。 每个规则最多可以有 20 个扩展，一个扩展就是一个意义相同的词。 例如，给定“猫”，与“猫咪”、“猫科动物”和“猫属”（猫的属）的关联将算作 3 个扩展。

| 资源 | 免费 | 基本 | S1 | S2 | S3 | S3-HD |L1 | L2 |
| -------- | -----|------ |----|----|----|-------|---|----|
| 最大同义词映射数 |3 |3|5 |10 个 |20 个 |20 个 | 10 个 | 10 个 |
| 每个映射的最大规则数 |5000 |20000|20000 |20000 |20000 |20000 | 20000 | 20000  |

## <a name="queries-per-second-qps"></a>每秒查询次数 (QPS)

每个客户必须独立制定 QPS 估计值。 索引大小和复杂性、查询大小和复杂性以及流量大小是 QPS 的主要决定因素。 当此类因素未知时，没有方法能提供有意义的估计值。

针对专用资源（基本层和标准层）上运行的服务进行计算时，估计值更可预测。 由于能够控制更多参数，因此可以更精确地评估 QPS。 有关如何进行估算的指导，请参阅 [Azure 认知搜索的性能和优化](search-performance-optimization.md)。

与“标准”层相比，“存储优化”层预计会有较低的查询吞吐量，较高的延迟。  估计会体验到的查询性能的方法与“标准”层相同。

<!-- ## Data limits (AI enrichment) -->

<!-- An [AI enrichment pipeline](cognitive-search-concept-intro.md) that makes calls to a Text Analytics resource for [entity recognition](cognitive-search-skill-entity-recognition.md), [key phrase extraction](cognitive-search-skill-keyphrases.md), [sentiment analysis](cognitive-search-skill-sentiment.md), and [language detection](cognitive-search-skill-language-detection.md) is subject to data limits. The maximum size of a record should be 50,000 characters as measured by [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length). If you need to break up your data before sending it to the sentiment analyzer, use the [Text Split skill](cognitive-search-skill-textsplit.md). -->

## <a name="throttling-limits"></a>限制

当系统接近峰值容量时，搜索查询和索引请求会受到限制。 对不同 API 的限制行为各不相同。 根据服务中的负载动态限制查询 API（搜索/建议/自动完成）和索引 API。 索引 API 的请求速率限制是静态的。 

索引相关操作的静态速率请求限制：

+ 列出索引 (GET /indexes)：每个搜索单位每秒限制为 5 个
+ 获取索引 (GET /indexes/myindex)：每个搜索单位每秒限制为 10 个
+ 创建索引 (POST /indexes)：每个搜索单位每分钟限制为 12 个
+ 创建或更新索引 (PUT /indexes/myindex)：每个搜索单位每秒限制为 6 个
+ 删除索引 (DELETE /indexes/myindex)：每个搜索单位每分钟限制为 12 个 

## <a name="api-request-limits"></a>API 请求限制
* 每个请求最大 16 MB <sup>1</sup>
* 最大 8 KB URL 长度
* 每个索引上传、合并或删除的批次最多包含 1000 个文档
* $Orderby 子句中最多 32 字段
* 最大搜索词大小为 UTF-8 编码文本的 32,766 字节（32 KB 减 2 个字节）

<sup>1</sup> 在 Azure 认知搜索中，请求主体受 16 MB 上限的约束，这会针对不受理论限制约束的单个字段或集合的内容施加实际限制（有关字段组合和限制的详细信息，请参阅[支持的数据类型](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)）。

## <a name="api-response-limits"></a>API 响应限制
* 每页搜索结果最多返回 1000 个文档
* 每个建议 API 请求最多返回 100 条建议

## <a name="api-key-limits"></a>API 密钥限制
API 密钥用于服务身份验证。 有两种类型。 管理密钥在请求标头中指定，并授予对服务的完全读写访问权限。 查询密钥是只读密钥并在 URL 上指定，并且通常分发给客户端应用程序。

* 每个服务最多 2 个管理密钥
* 每个服务最多 50 个查询密钥