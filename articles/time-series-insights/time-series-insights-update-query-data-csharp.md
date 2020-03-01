---
title: 使用 C# 从预览版环境查询数据 - Azure 时序见解 | Microsoft Docs
description: 了解如何使用 C# 编写的应用从 Azure 时序见解环境查询数据。
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: v-junlch
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/19/2020
ms.custom: seodec18
ms.openlocfilehash: 5c220d5c92f64b13d8ee867c70b02c108cd1e2ae
ms.sourcegitcommit: f5bc5bf51a4ba589c94c390716fc5761024ff353
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77494437"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>使用 C# 查询 Azure 时序见解预览版环境中的数据

本 C# 示例演示了如何在 Azure 时序见解预览版环境中通过[预览版数据访问 API](https://docs.microsoft.com/rest/api/time-series-insights/preview) 查询数据。

> [!TIP]
> 查看 [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample) 上的预览版 C# 代码示例。

## <a name="summary"></a>摘要

下面的示例代码演示了以下功能：

* [Azure AutoRest](https://github.com/Azure/AutoRest) 支持 SDK 自动生成。
* 如何使用 [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) 通过 Azure Active Directory 获取访问令牌。
* 如何在后续数据访问 API 请求的 `Authorization` 标头中传递所获取的该访问令牌。 
* 该示例提供了一个控制台界面，其中演示了如何对以下项发出 HTTP 请求：

    * [预览版环境 API](https://docs.microsoft.com/rest/api/time-series-insights/preview#preview-environments-apis)
        * [获取环境可用性 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability) 和[获取事件架构 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema)
    * [预览版查询 API](https://docs.microsoft.com/rest/api/time-series-insights/preview#query-apis)
        * [获取事件 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents)、[获取系列 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries) 和[获取聚合系列 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
    * [时序模型 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
        * [获取层次结构 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/get) 和[层次结构批处理 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/executebatch)
        * [获取类型 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/get) 和[类型批处理 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/executebatch)
        * [获取实例 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/get) 和[实例批处理 API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/executebatch)
* 高 [搜索](https://docs.microsoft.com/rest/api/time-series-insights/preview#search-features)和 [TSX](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) 功能。

## <a name="prerequisites-and-setup"></a>先决条件和设置

在编译和运行示例代码之前，请完成以下步骤：

1. [预配预览版 Azure 时序见解](/time-series-insights/time-series-insights-update-how-to-manage#create-the-environment)环境。
1. 为 Azure Active Directory 配置 Azure 时序见解环境，如[身份验证和授权](time-series-insights-authentication-and-authorization.md)中所述。 
1. 按照 [Readme.md](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/Readme.md) 中指定的方式运行 [GenerateCode.bat](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/GenerateCode.bat)，以生成时序见解预览版客户端依赖项。
1. 在 Visual Studio 中打开 `TSIPreviewDataPlaneclient.sln` 解决方案，并将 `DataPlaneClientSampleApp` 设置为默认项目。
1. 使用[下文](#project-dependencies)所述的步骤安装所需的项目依赖项，并将示例编译为可执行 `.exe` 文件。
1. 双击 `.exe` 文件来运行该文件。

## <a name="project-dependencies"></a>项目依赖项

建议使用最新版 Visual Studio：

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) - 版本 16.4.2+

示例代码具有几个必需的依赖项，可以在 [packages.config](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClientSampleApp/packages.config) 文件中查看这些依赖项。

在 Visual Studio 2019 中，通过选择“生成” > “生成解决方案”选项来下载程序包。   

或者，使用 [NuGet 2.12+](https://www.nuget.org/) 添加这些程序包。 例如：

* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 4.5.1`

## <a name="c-sample-code"></a>C# 示例代码

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Threading.Tasks;
using Microsoft.Azure;
using Microsoft.Azure.TimeSeriesInsights;
using Microsoft.Azure.TimeSeriesInsights.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
using Newtonsoft.Json;
using DateTimeRange = Microsoft.Azure.TimeSeriesInsights.Models.DateTimeRange;

namespace DataPlaneSampleApp
{
    public sealed class Program
    {
        private const string ResourceUri = "https://api.timeseries.azure.com/";
        private const string PowerShellAadClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string AzureActiveDirectoryLoginUrl = "https://login.windows.net";
        private const string MicrosoftTenantId = "72f988bf-86f1-41af-91ab-2d7cd011db47";

        // This can be found under time series environment resource on Azure portal under "Data Access FQDN".
        // This is the "Contoso Wind Farm" environment that can be found at - "https://insights.timeseries.azure.com/preview/samples"
        private const string EnvironmentFqdn = "10000000-0000-0000-0000-100000000109.env.timeseries.azure.com";

        // Select a timeSeriesId from the environment.
        private static readonly object[] TimeSeriesId = new object[] { "2da181d7-8346-4cf2-bd94-a17742237429" };

        // Select a search span for the query.
        private static readonly DateTimeRange SearchSpan = new DateTimeRange(new DateTime(2017, 12, 31).ToUniversalTime(), new DateTime(2018, 01, 01).ToUniversalTime());
        private static readonly Uri RedirectUri = new Uri("urn:ietf:wg:oauth:2.0:oob");

        private static TimeSeriesInsightsClient _client;

        static void Main(string[] args)
        {
            System.Net.ServicePointManager.SecurityProtocol |= SecurityProtocolType.Tls11 | SecurityProtocolType.Tls12;

            _client = GetTimeSeriesInsightsClientAsync().Result;

            while (true)
            {
                RunOperationsLoopAsync().Wait();
            }
        }

        private class Operation
        {
            public string Description { get; }
            public Func<Task> Action { get; }

            public Operation(string description, Func<Task> action)
            {
                Description = description;
                Action = action;
            }
        }

        private static async Task RunOperationsLoopAsync()
        {
            Console.WriteLine();
            Console.WriteLine("Choose one of the operations:");
            Console.WriteLine("Environment: {0}", EnvironmentFqdn);
            Console.WriteLine("TimeSeriesId: {0}", TimeSeriesId);
            Console.WriteLine("SearchSpan: From: {0}, To: {1}", SearchSpan.FromProperty, SearchSpan.To);

            foreach (var op in Operations)
            {
                Console.WriteLine($"{op.Key}\t{op.Value.Description}");
            }

            try
            {
                Console.Write("Select an operation to run: ");
                int selection = int.Parse(Console.ReadLine());

                await Operations[selection].Action();
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
            }
        }

        private static readonly Dictionary<int, Operation> Operations = new Dictionary<int, Operation>
        {
            [0] = new Operation("Availability", GetAvailabilityAsync),
            [1] = new Operation("EventSchema", GetEventSchemaAsync),
            [2] = new Operation("Query/AggregateSeries", RunAggregateSeriesAsync),
            [3] = new Operation("Query/GetSeries", RunGetSeriesAsync),
            [4] = new Operation("Query/GetEvents", RunGetEventsAsync),
            [5] = new Operation("Query/GetEventsWithProjectedProperties", RunGetEventsWithProjectedPropertiesAsync),
            [6] = new Operation("GetInstances", RunGetInstancesAsync),
            [7] = new Operation("InstancesBatch", RunInstancesBatchAsync),
            [8] = new Operation("GetTypes", RunGetTypesAsync),
            [9] = new Operation("TypesBatch", RunTypesBatchAsync),
            [10] = new Operation("GetHierarchies", RunGetHierarchiesAsync),
            [11] = new Operation("HierarchiesBatch", RunHierarchiesBatchAsync),
            [12] = new Operation("Search", RunSearchBatchAsync),
        };

                private static async Task RunSearchBatchAsync()
        {
            SearchInstancesRequest searchRequest = new SearchInstancesRequest(
                "GearboxSensor",
                new List<string>() { "Contoso WindFarm Hierarchy" },
                new SearchInstancesParameters(true, new InstancesSortParameter(InstancesSortBy.Rank), true, 10),
                new SearchInstancesHierarchiesParameters(
                    new HierarchiesExpandParameter(HierarchiesExpandKind.UntilChildren),
                    new HierarchiesSortParameter(HierarchiesSortBy.CumulativeInstanceCount),
                    10));
            SearchInstancesResponsePage searchResponse = await _client.TimeSeriesInstances.SearchAsync(searchRequest);
            PrintResponse(searchResponse);
        }

        private static void PrintResponse(SearchInstancesResponsePage searchResponse)
        {
            Console.WriteLine("Search Results: {0}", JsonConvert.SerializeObject(searchResponse, Formatting.Indented));
        }

        private static async Task RunHierarchiesBatchAsync()
        {
            HierarchiesBatchResponse hierarchies =
                await _client.TimeSeriesHierarchies.ExecuteBatchAsync(new HierarchiesBatchRequest(get: new HierarchiesRequestBatchGetDelete(names: new List<string>() { "Contoso WindFarm Hierarchy" })));

            PrintResponse(hierarchies.Get.First().Hierarchy);
        }

        private static async Task RunGetHierarchiesAsync()
        {
            string continuationToken = null;
            do
            {
                GetHierarchiesPage hierarchies = await _client.TimeSeriesHierarchies.GetAsync(continuationToken: continuationToken);

                PrintResponse(hierarchies.Hierarchies);
                continuationToken = hierarchies.ContinuationToken;
            }
            while (continuationToken != null);
        }

        private static void PrintResponse(IList<TimeSeriesHierarchy> hierarchies)
        {
            Console.WriteLine("Hierarchies");
            foreach (TimeSeriesHierarchy hierarchy in hierarchies)
            {
                PrintResponse(hierarchy);
            }
        }

        private static void PrintResponse(TimeSeriesHierarchy hierarchy)
        {
            Console.WriteLine("Hierarchy:");
            Console.WriteLine("  ID: {0}", hierarchy.Id);
            Console.WriteLine("  Name: {0}", hierarchy.Name);
            Console.WriteLine("  Source: {0}", string.Join(" > ", hierarchy.Source.InstanceFieldNames));
        }

        private static async Task RunTypesBatchAsync()
        {
            TypesBatchResponse types = await _client.TimeSeriesTypes.ExecuteBatchAsync(
                new TypesBatchRequest(
                    get: new TypesRequestBatchGetOrDelete(typeIds: new List<Guid?>() { Guid.Parse("1BE09AF9-F089-4D6B-9F0B-48018B5F7393") })));

            PrintResponse(types.Get.First().TimeSeriesType);
        }

        private static async Task RunGetTypesAsync()
        {
            string continuationToken = null;
            do
            {
                GetTypesPage types = await _client.TimeSeriesTypes.GetAsync(continuationToken: continuationToken);

                PrintResponse(types.Types);
                continuationToken = types.ContinuationToken;
            }
            while (continuationToken != null);
        }

        private static void PrintResponse(IList<TimeSeriesType> types)
        {
            Console.WriteLine("Types: ");
            foreach (TimeSeriesType type in types)
            {
                PrintResponse(type);
            }
        }

        private static void PrintResponse(TimeSeriesType type)
        {
            Console.WriteLine("Type ID: {0}", type.Id);
            Console.WriteLine("  Name: {0}", type.Name);
            Console.WriteLine("  Description: {0}", type.Description);
            foreach (KeyValuePair<string, Variable> variable in type.Variables)
            {
                Console.WriteLine("  Variable: {0}", variable.Key);
                Console.WriteLine("    Filter: {0}", variable.Value.Filter?.TsxProperty);
                var numericVariable = variable.Value as NumericVariable;
                if (numericVariable != null)
                {
                    Console.WriteLine("    Value: {0}", numericVariable.Value?.TsxProperty);
                    Console.WriteLine("    Aggregation: {0}", numericVariable.Aggregation?.TsxProperty);
                }

                var aggregateVariable = variable.Value as AggregateVariable;
                if (aggregateVariable != null)
                {
                    Console.WriteLine("    Aggregation: {0}", aggregateVariable.Aggregation?.TsxProperty);
                }
            }
        }

        private static async Task RunInstancesBatchAsync()
        {
            InstancesBatchResponse instances = await _client.TimeSeriesInstances.ExecuteBatchAsync(
                new InstancesBatchRequest(get: new InstancesRequestBatchGetOrDelete(new IList<object>[] { TimeSeriesId })));

            PrintResponse(instances.Get.First().Instance);
        }

        private static async Task RunGetInstancesAsync()
        {
            string continuationToken = null;

            // Limit the total instances received.
            int limit = 1000;
            int totalInstanceCount = 0;
            TimeSeriesInstance firstInstance = null;
            do
            {
                GetInstancesPage instancesPage = await _client.TimeSeriesInstances.GetAsync(continuationToken: continuationToken);

                if (instancesPage.Instances != null)
                {
                    totalInstanceCount += instancesPage.Instances.Count;

                    Console.WriteLine("Received instances : {0}", totalInstanceCount);

                    if (firstInstance == null)
                    {
                        firstInstance = instancesPage.Instances.FirstOrDefault();
                    }
                }

                continuationToken = instancesPage.ContinuationToken;
            }
            while (continuationToken != null && totalInstanceCount < limit);

            Console.WriteLine("First Instance:");
            PrintResponse(firstInstance);
        }

        private static void PrintResponse(TimeSeriesInstance timeSeriesInstance)
        {
            Console.WriteLine("Time Series Instance");
            Console.WriteLine("  Time Series ID: {0}", string.Join(", ", timeSeriesInstance.TimeSeriesId));
            Console.WriteLine("  Description: {0}", timeSeriesInstance.Description);
            Console.WriteLine("  Type ID: {0}", timeSeriesInstance.TypeId);
            Console.WriteLine("  Hierarchy IDs: \n    {0}\n", timeSeriesInstance.HierarchyIds != null ? string.Join("\n    ", timeSeriesInstance.HierarchyIds) : "null");
            Console.WriteLine("  Instance Fields: \n    {0}\n", timeSeriesInstance.InstanceFields != null ? string.Join("\n    ", timeSeriesInstance.InstanceFields.Select(i => $"{i.Key}: {i.Value}")) : "null");
        }

        private static async Task GetAvailabilityAsync()
        {
            AvailabilityResponse availability = await _client.Query.GetAvailabilityAsync();
            PrintResponse(availability.Availability);
        }

        private static async Task GetEventSchemaAsync()
        {
            EventSchema eventSchema = await _client.Query.GetEventSchemaAsync(new GetEventSchemaRequest(SearchSpan));
            PrintResponse(eventSchema);
        }

        private static async Task RunAggregateSeriesAsync()
        {
            string continuationToken = null;
            do
            {
                QueryResultPage queryResponse = await _client.Query.ExecuteAsync(
                    new QueryRequest(
                        aggregateSeries: new AggregateSeries(
                            timeSeriesId: TimeSeriesId,
                            searchSpan: SearchSpan,
                            filter: null,
                            interval: TimeSpan.FromMinutes(5),
                            projectedVariables: new[] { "Min_Numeric", "Max_Numeric", "Sum_Numeric", "Avg_Numeric", "First_Numeric", "Last_Numeric", "Count_Aggregate" },
                            inlineVariables: new Dictionary<string, Variable>()
                            {
                                ["Min_Numeric"] = new NumericVariable(
                                    value: new Tsx("$event.data"),
                                    aggregation: new Tsx("min($value)")),
                                ["Max_Numeric"] = new NumericVariable(
                                    value: new Tsx("$event.data"),
                                    aggregation: new Tsx("max($value)")),
                                ["Sum_Numeric"] = new NumericVariable(
                                    value: new Tsx("$event.data"),
                                    aggregation: new Tsx("sum($value)")),
                                ["Avg_Numeric"] = new NumericVariable(
                                    value: new Tsx("$event.data"),
                                    aggregation: new Tsx("avg($value)")),
                                ["First_Numeric"] = new NumericVariable(
                                    value: new Tsx("$event.data"),
                                    aggregation: new Tsx("first($value)")),
                                ["Last_Numeric"] = new NumericVariable(
                                    value: new Tsx("$event.data"),
                                    aggregation: new Tsx("last($value)")),
                                ["Count_Aggregate"] = new AggregateVariable(
                                    aggregation: new Tsx("count()"))
                            })),
                    continuationToken: continuationToken);

                PrintResponse(queryResponse);

                continuationToken = queryResponse.ContinuationToken;
            }
            while (continuationToken != null);
        }

        private static async Task RunGetSeriesAsync()
        {
            string continuationToken = null;
            do
            {
                QueryResultPage queryResponse = await _client.Query.ExecuteAsync(
                    new QueryRequest(
                        getSeries: new GetSeries(
                            timeSeriesId: TimeSeriesId,
                            searchSpan: SearchSpan,
                            filter: null,
                            projectedVariables: new[] { "Value" },
                            inlineVariables: new Dictionary<string, Variable>()
                            {
                                ["Value"] = new NumericVariable(
                                    value: new Tsx("$event.data"),
                                    aggregation: new Tsx("avg($value)"))
                            })),
                    continuationToken: continuationToken);

                PrintResponse(queryResponse);

                continuationToken = queryResponse.ContinuationToken;
            }
            while (continuationToken != null);
        }

        private static async Task RunGetEventsAsync()
        {
            string continuationToken = null;
            do
            {
                QueryResultPage queryResponse = await _client.Query.ExecuteAsync(
                    new QueryRequest(
                        getEvents: new GetEvents(
                            timeSeriesId: TimeSeriesId,
                            searchSpan: SearchSpan,
                            filter: null)),
                    continuationToken: continuationToken);

                PrintResponse(queryResponse);

                continuationToken = queryResponse.ContinuationToken;
            }
            while (continuationToken != null);
        }

        private static async Task RunGetEventsWithProjectedPropertiesAsync()
        {
            string continuationToken = null;
            do
            {
                QueryResultPage queryResponse = await _client.Query.ExecuteAsync(
                    new QueryRequest(
                        getEvents: new GetEvents(
                            timeSeriesId: TimeSeriesId,
                            searchSpan: SearchSpan,
                            filter: null,
                            projectedProperties: new List<EventProperty>() { new EventProperty("data", PropertyTypes.Double) })),
                    continuationToken: continuationToken);

                PrintResponse(queryResponse);

                continuationToken = queryResponse.ContinuationToken;
            }
            while (continuationToken != null);
        }

        private static async Task<TimeSeriesInsightsClient> GetTimeSeriesInsightsClientAsync()
        {
            AuthenticationContext context = new AuthenticationContext($"{AzureActiveDirectoryLoginUrl}/{MicrosoftTenantId}", TokenCache.DefaultShared);
            AuthenticationResult authenticationResult = await context.AcquireTokenAsync(ResourceUri, PowerShellAadClientId, RedirectUri, new PlatformParameters(PromptBehavior.Auto));

            TokenCloudCredentials tokenCloudCredentials = new TokenCloudCredentials(authenticationResult.AccessToken);
            ServiceClientCredentials serviceClientCredentials = new TokenCredentials(tokenCloudCredentials.Token);

            TimeSeriesInsightsClient timeSeriesInsightsClient = new TimeSeriesInsightsClient(credentials: serviceClientCredentials)
            {
                EnvironmentFqdn = EnvironmentFqdn
            };
            return timeSeriesInsightsClient;
        }

        private static void PrintResponse(QueryResultPage queryResultPage)
        {
            Console.WriteLine();
            Console.WriteLine("Query result page:");
            Console.WriteLine();

            if (queryResultPage.Properties != null && queryResultPage.Timestamps != null)
            {
                Console.Write("timestamp,");
                Console.WriteLine(string.Join(",", queryResultPage.Properties.Select(v => v.Name)));
                int i = 0;
                foreach (DateTime? bodyTimestamp in queryResultPage.Timestamps)
                {
                    List<string> row = new List<string>();

                    row.Add(bodyTimestamp?.ToString("o"));
                    foreach (PropertyValues propertyValues in queryResultPage.Properties)
                    {
                        row.Add(propertyValues.Values[i] == null ? "null" : propertyValues.Values[i].ToString());
                    }

                    Console.WriteLine(string.Join(",", row));
                    i++;
                }
                Console.WriteLine();
            }
            else
            {
                Console.WriteLine("Result page is empty.");
            }
        }

        private static void PrintResponse(Availability availability)
        {
            Console.WriteLine("Availability:");

            if (availability != null)
            {

                Console.WriteLine("  IntervalSize: {0}", availability.IntervalSize);
                Console.WriteLine("  Range: From: {0}, To: {1}", availability.Range.FromProperty, availability.Range.To);
                Console.WriteLine("  Distribution:");
                foreach (KeyValuePair<string, int?> dist in availability.Distribution)
                {
                    Console.WriteLine("    {0}: {1}", dist.Key, dist.Value);
                }
            }
            else
            {
                Console.WriteLine("Environment is empty.");
                return;
            }

            Console.WriteLine();
        }

        private static void PrintResponse(EventSchema eventSchema)
        {
            Console.WriteLine("EventSchema");
            foreach (EventProperty eventProperty in eventSchema.Properties)
            {
                Console.WriteLine("  {0}: {1}", eventProperty.Name, eventProperty.Type);
            }

            Console.WriteLine();
        }
    }
}
```

> [!NOTE]
> * 可以在不改变默认环境变量的情况下执行代码示例。
> * 此代码示例将编译为 .NET 可执行控制台应用。

## <a name="next-steps"></a>后续步骤

- 若要详细了解查询，请阅读[查询 API 参考](https://docs.microsoft.com/rest/api/time-series-insights/preview-query)。

- 阅读如何[使用客户端 SDK 将 JavaScript 应用连接到时序见解](https://github.com/microsoft/tsiclient)。

