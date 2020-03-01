---
title: 收集日志数据
titleSuffix: Azure Cognitive Search
description: 通过启用诊断设置来收集和分析日志数据，然后使用 Kusto 查询语言浏览结果。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 02/11/2020
ms.date: 03/02/2020
ms.openlocfilehash: 90356e49c05d84e920b12ef6d5007397d5209c97
ms.sourcegitcommit: 094c057878de233180ff3b3a3e3c19bc11c81776
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77504187"
---
# <a name="collect-and-analyze-log-data-for-azure-cognitive-search"></a>收集和分析 Azure 认知搜索的日志数据

诊断或操作日志提供 Azure 认知搜索的详细操作的见解，可用于监视服务和工作负荷流程。 在内部，日志会短时存在于后端，但足以进行调查和分析（如果你提交了支持票证）。 但是，如果想要自我掌控操作数据，则应配置诊断设置以指定要从何处收集日志记录信息。 

设置日志将有助于诊断和保留操作历史记录。 启用日志记录后，可以运行查询或生成报告进行有条理的分析。

下表列举了用于收集和保留数据的选项。

| 资源 | 用途 |
|----------|----------|
| [使用 Blob 存储进行存档](https://docs.azure.cn/storage/blobs/storage-blobs-overview) | 记录的事件和查询指标，基于下面的架构。 事件记录到 Blob 容器并存储在 JSON 文件中。 日志可以精确到小时/分钟，对于调查特定的事件非常有用，但不适合用于无目标性的调查。 使用 JSON 编辑器查看日志文件。|
| [流式传输到事件中心](https://docs.azure.cn/event-hubs/) | 记录的事件和查询指标，基于本文中记录的架构。 对于很大的日志，请选择此项作为备用数据收集服务。 |

Azure Monitor 日志和 Blob 存储均以免费服务的形式提供，让你可以在 Azure 订阅的生存期内免费试用它。 Application Insights 可以免费注册和使用，前提是应用程序数据大小不超出特定限制（有关详细信息，请参阅[定价页](https://www.azure.cn/pricing/details/monitor/)）。

## <a name="prerequisites"></a>必备条件

如果使用 Log Analytics 或 Azure 存储，可以提前创建资源。

+ [创建 Log Analytics 工作区](https://docs.azure.cn/azure-monitor/learn/quick-create-workspace)

+ 如果需要日志存档，请[创建存储帐户](https://docs.azure.cn/storage/common/storage-quickstart-create-account)。

## <a name="create-a-log"></a>创建日志

诊断设置定义数据收集。 某项设置可以指定收集方式和收集的内容。 

1. 在“监视”下，选择“诊断设置”   。

   ![诊断设置](./media/search-monitor-usage/diagnostic-settings.png "诊断设置")

1. 选择“+ 添加诊断设置” 

1. 选择要导出的数据：日志和/或指标。 可以收集存储帐户、Log Analytics 工作区中的数据，或将其流式传输到事件中心。

   建议使用 Log Analytics，因为可以在门户中查询工作区。

   如果还使用了 Blob 存储，则在导出日志数据时，会根据需要创建容器和 Blob。

   ![配置数据收集](./media/search-monitor-usage/configure-storage.png "配置数据收集")

1. 保存设置。

1. 通过创建或删除对象（创建日志事件）以及通过提交查询（生成指标）进行测试。 

在 Blob 存储中，仅当存在要记录或度量的活动时，才会创建容器。 将数据复制到存储帐户时，数据会被格式化为 JSON 并置于两个容器中：

* insights-logs-operationlogs：用于搜索流量日志
* insights-metrics-pt1m：用于指标

**一个小时后，容器才会出现在 Blob 存储中。每个容器每小时会有一个 Blob。**

发生活动的每个小时都会存档日志。 以下路径是在 2020 年 1 月 12 日上午 9:00 创建的一个日志文件示例。 其中的每个 `/` 都是一个文件夹：`resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2020/m=01/d=12/h=09/m=00/name=PT1H.json`

## <a name="log-schema"></a>日志架构

包含 Azure 认知搜索日志数据的数据结构遵从以下架构。 

对于 Blob 存储，每个 Blob 都有一个名为 **records** 的根对象，其中包含一系列日志对象。 每个 Blob 包含同一小时内发生的所有操作的记录。

下表是诊断日志记录常用字段的部分列表。

| 名称 | 类型 | 示例 | 注释 |
| --- | --- | --- | --- |
| timeGenerated |datetime |"2018-12-07T00:00:43.6872559Z" |操作的时间戳 |
| ResourceId |string |“/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/> MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE” |ResourceId |
| operationName |string |“Query.Search” |操作的名称 |
| operationVersion |string |“2019-05-06” |使用的 api-version |
| category |string |“OperationLogs” |constant |
| resultType |string |“Success” |可能的值：Success 或 Failure |
| resultSignature |int |200 |HTTP 结果代码 |
| durationMS |int |50 |操作持续时间，以毫秒为单位 |
| properties |object |请参阅下表 |包含特定于操作的数据的对象 |

### <a name="properties-schema"></a>属性架构

以下属性特定于 Azure 认知搜索。

| 名称 | 类型 | 示例 | 注释 |
| --- | --- | --- | --- |
| Description_s |string |“GET /indexes('content')/docs” |操作的终结点 |
| Documents_d |int |42 |处理的文档数目 |
| IndexName_s |string |"test-index" |与操作关联的索引名称 |
| Query_s |string |“?search=AzureSearch&$count=true&api-version=2019-05-06” |查询参数 |

## <a name="metrics-schema"></a>度量值架构

按一分钟间隔捕获和度量查询请求的指标。 每个度量值都会显示每分钟的最小、最大和平均值。 有关详细信息，请参阅[监视查询请求](search-monitor-queries.md)。

| 名称 | 类型 | 示例 | 注释 |
| --- | --- | --- | --- |
| ResourceId |string |“/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/>MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE” |资源 ID |
| metricName |string |“Latency” |度量值名称 |
| time |datetime |"2018-12-07T00:00:43.6872559Z" |操作的时间戳 |
| average |int |64 |指标时间间隔内原始样本的平均值，单位为秒或百分比，具体取决于指标。 |
| 最小值 |int |37 |指标时间间隔内原始样本的最小值，单位为秒。 |
| 最大值 |int |78 |指标时间间隔内原始样本的最大值，单位为秒。  |
| total |int |258 |指标时间间隔内原始样本的总计值，单位为秒。  |
| count |int |4 |在一分钟间隔内从节点发出到日志的指标数。  |
| timegrain |string |“PT1M” |采用 ISO 8601 的指标时间粒度。 |

查询往往在若干毫秒内即可完成执行，因此，指标中仅显示以秒度量的查询，例如 QPS。

对于“每秒搜索查询数”指标，最小值是该分钟内已注册的每秒搜索查询次数最低值。  最大值也是如此。 平均值是一分钟内的聚合值。 例如，在一分钟内可能出现如下所述的模式：有 1 秒出现高负载（这是 SearchQueriesPerSecond 的最大值），紧接着有 58 秒的平均负载，最后 1 秒只有 1 个查询（这是最小值）。

对于“受限制的搜索查询百分比”、最小值、最大值、平均值和总计，全都具有相同的值：在一分钟内的搜索查询总数中，已限制搜索查询百分比  。

## <a name="view-log-files"></a>查看日志文件

Blob 存储用于存档日志文件。 可以使用任何 JSON 编辑器来查看日志文件。 如果没有编辑器，建议使用 [Visual Studio Code](https://code.visualstudio.com/download)。

1. 在 Azure 门户中打开存储帐户。 

2. 在左侧导航窗格中，单击“Blob”  。 此时会看到 **insights-logs-operationlogs** 和 **insights-metrics-pt1m**。 这些容器是在将日志数据导出到 Blob 存储时由 Azure 认知搜索创建的。

3. 单击文件夹层次结构，直至找到 .json 文件。  通过上下文菜单来下载文件。

待文件下载完以后，在 JSON 编辑器中将其打开即可查看内容。

## <a name="next-steps"></a>后续步骤

如果尚未这样做，请查看搜索服务监视基础知识，以全方面地了解监督功能。

> [!div class="nextstepaction"]
> [监视 Azure 认知搜索中的操作和活动](search-monitor-usage.md)