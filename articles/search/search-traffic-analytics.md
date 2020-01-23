---
title: 基于搜索流量分析数据进行报告
titleSuffix: Azure Cognitive Search
description: 为 Azure 认知搜索启用搜索流量分析，使用 Application Insights 收集遥测数据和用户启动的事件，然后在 Power BI 报告中对结果进行分析。
author: HeidiSteen
manager: nitinme
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 12/11/2019
ms.date: 01/17/2020
ms.openlocfilehash: 8ed8ce8f8cde4e475ee632d732f07297271420e2
ms.sourcegitcommit: 94e1c9621b8f81a7078f1412b3a73281d0a8668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76123416"
---
# <a name="implement-search-traffic-analytics-in-azure-cognitive-search"></a>在 Azure 认知搜索中实现搜索流量分析

搜索流量分析是用于为搜索服务实现反馈循环的模式。 目标是收集有关用户启动的单击事件和键盘输入的遥测数据。 使用此信息，你可以确定搜索解决方案的有效性，包括热门搜索词、点击率以及哪些查询输入产生了零个结果。

此模式依赖于 [Application Insights](https://docs.azure.cn/azure-monitor/app/app-insights-overview)（[Azure Monitor](https://docs.azure.cn/azure-monitor/) 的一项功能）来收集用户数据。 你还需要向客户端代码中添加检测，如本文中所述。 最后，你将需要一个报告机制来分析数据。 建议使用 Power BI，但你可以使用连接到 Application Insights 的任何工具。

> [!NOTE]
> 本文中所述的模式适用于客户端生成的高级方案和点击流数据。 另外，你还可以报告搜索服务生成的日志信息。 有关服务日志报告的详细信息，请参阅[监视资源消耗和查询活动](search-monitor-usage.md)。

## <a name="identify-relevant-search-data"></a>标识相关的搜索数据

若要拥有有用的搜索指标，请务必记录来自搜索应用程序用户的某些信号。 这些信号意味着用户感兴趣的内容以及他们认为与其需求相关的内容。

搜索流量分析需要两个信号：

+ 用户生成的搜索事件：只有用户启动的搜索查询才需要关注。 用于填充 Facet、附加内容或任何内部信息的搜索查询都不重要，它们会扭曲结果并造成结果有偏差。

+ 用户生成的单击事件：本文档中的单击是指用户选择从搜索查询返回的特定搜索结果。 一次单击通常意味着文档是特定搜索查询的相关结果。

通过使用相关性 ID 链接搜索和单击事件，可以分析应用程序上的用户的行为。 如果仅使用搜索流量日志，则无法获取这些搜索见解。

## <a name="add-search-traffic-analytics"></a>添加搜索流量分析

前一部分中提到的信号必须在用户与搜索应用程序交互时从该搜索应用程序中收集。 Application Insights 是一个可扩展的监视解决方案，可用于多个平台，具有灵活的检测选项。 使用 Application insights 可以充分利用由 Azure 认知搜索创建的 Power BI 搜索报表，从而使数据分析更加容易。

在 Azure 认知搜索服务的[门户](https://portal.azure.cn)页中，“搜索流量分析”页面包含一个用于遵循此遥测模式的速查表。 还可以选择或创建 Application Insights 资源，并查看必需的数据，所有这些全都在一个位置完成。

![“搜索流量分析”说明][1]

## <a name="1---select-a-resource"></a>1 - 选择资源

需要选择要使用的 Application Insights 资源，如果还没有 Application Insights 资源，则需要创建一个。 可以使用已在使用的资源记录所需的自定义事件。

创建新的 Application Insights 资源时，此方案对所有应用程序类型都有效。 选择一个最适合所用平台的资源。

为应用程序创建遥测客户端时需要使用检测密钥。 可以从 Application Insights 门户仪表板获取该密钥，也可以从“搜索流量分析”页获取它，只需选择要使用的实例即可。

## <a name="2---add-instrumentation"></a>2 - 添加检测

在此步骤中，将使用以上步骤中创建的 Application Insights 资源检测自己的搜索应用程序。 此过程有四个步骤：

**步骤 1：创建一个遥测客户端**

这是将事件发送到 Application Insights 资源的对象。

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

对于其他语言和平台，请参阅完整的[列表](https://docs.azure.cn/azure-monitor/app/platforms)。

**步骤 2：请求用于关联的搜索 ID**

为了将搜索请求与单击相关联，必须具有一个将这两个不同事件关联起来的相关性 ID。 使用标头请求搜索 ID 时，Azure 认知搜索将提供该 ID：

*C#*

    // This sample uses the .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<SearchServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**步骤 3：记录搜索事件**

每当用户发出搜索请求时，应使用 Application Insights 自定义事件上的以下架构，将该请求作为搜索事件进行记录：

**SearchServiceName**：(string) 搜索服务名称 **SearchId**：(guid) 搜索查询的唯一标识符（进入搜索响应） **IndexName**：(string) 要查询的搜索服务索引 **QueryTerms**：(string) 用户输入的搜索词 **ResultCount**：(int) 返回的文档数（进入搜索响应）**ScoringProfile**：(string) 所用计分概要文件的名称（如果有计分概要文件）

> [!NOTE]
> 请通过向搜索查询添加 $count=true 来请求用户生成查询的计数。 请在[此处](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)查看详细信息
>

> [!NOTE]
> 请记住仅记录由用户生成的搜索查询。
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**步骤 4：记录单击事件**

每次用户单击文档，都是一个必须记录以用于搜索分析的信号。 使用 Application Insights 自定义事件可利用下面的架构来记录这些事件：

**ServiceName**：(string) 搜索服务名称 **SearchId**：(guid) 相关搜索查询的唯一标识符 **DocId**：(string) 文档标识符 **Position**：(int) 搜索结果页中文档的排名

> [!NOTE]
> Position 指的是应用程序中的基数顺序。 可以随意设置此数字以用于比较，只要它始终相同。
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.trackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

## <a name="3---analyze-in-power-bi"></a>3 - 在 Power BI 中进行分析

检测到应用并确认应用程序已正确连接到 Application Insights 后，下载一个预定义的报表模板以在 Power BI Desktop 中分析数据。 该报告包含预定义的图表和表，它们可用于分析为搜索流量分析捕获的其他数据。 

1. 在 Azure 认知搜索仪表板左侧导航窗格中，在“设置”  下，单击“搜索流量分析”  。

2. 在“搜索流量分析”  页面上，在步骤 3 中，单击“获取 Power BI Desktop”  以安装 Power BI。

   ![获取 Power BI 报表](./media/search-traffic-analytics/get-use-power-bi.png "获取 Power BI 报表")

2. 在同一页面上，单击“下载 Power BI 报表”  。

3. 该报表将在 Power BI Desktop 中打开，并且会提示你连接到 Application Insights 并提供凭据。 可以在你的 Application Insights 资源的 Azure 门户页面中找到连接信息。 对于凭据，请提供用于门户登录的相同用户名和密码。

   ![连接到 Application Insights](./media/search-traffic-analytics/connect-to-app-insights.png "连接到 Application Insights")

4. 单击“加载”。 

该报表包含图表和表，可帮助你做出更明智的决策来提高搜索性能和相关性。

指标包括以下各项：

* 搜索量和最常用术语-文档对：导致同一文档被单击的词，按单击次数排序。
* 无单击的搜索：查询次数最多但未记录任何单击的词

以下屏幕截图显示了用于分析搜索流量的内置报表和图表。

![用于 Azure 认知搜索的 Power BI 仪表板](./media/search-traffic-analytics/azuresearch-powerbi-dashboard.png "用于 Azure 认知搜索的 Power BI 仪表板")

## <a name="next-steps"></a>后续步骤
检测搜索应用程序，以获取提供深入见解的有关搜索服务的强大数据。

你可以查找有关 [Application Insights](https://docs.azure.cn/azure-monitor/app/app-insights-overview) 的更多信息并访问[定价页面](https://www.azure.cn/pricing/details/monitor/)来详细了解其各种服务层级。

了解有关创建出色报告的详细信息。 有关详细信息，请参阅 [Power BI Desktop 入门](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)。

<!--Image references-->
[1]: ./media/search-traffic-analytics/azuresearch-trafficanalytics.png
[2]: ./media/search-traffic-analytics/azuresearch-appinsightsdata.png
[3]: ./media/search-traffic-analytics/azuresearch-pbitemplate.png
