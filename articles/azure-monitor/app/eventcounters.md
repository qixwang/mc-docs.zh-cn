---
title: Application Insights 中的事件计数器 | Microsoft Docs
description: 监视 Application Insights 中的系统和自定义的 .NET/.NET Core EventCounters。
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 05/25/2020
ms.openlocfilehash: 94d11a4707c188b6cb004a948deb1b69be9a443d
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200213"
---
# <a name="eventcounters-introduction"></a>EventCounters 简介

`EventCounter` 用于发布和使用计数器或统计信息的 .NET/.NET Core 机制。 [本文档](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.Tracing/documentation/EventCounterTutorial.md)概述了 `EventCounters` 以及如何发布及使用它们的示例。 所有 OS 平台（Windows、Linux 和 macOS）均支持 EventCounters。 可以将其视为仅在 Windows 系统中受支持的 [PerformanceCounters](https://docs.microsoft.com/dotnet/api/system.diagnostics.performancecounter) 的跨平台等效项。

尽管用户可以根据需要发布任何自定义 `EventCounters`，但 .NET Core 3.0 运行时默认会发布一组此类计数器。 该文档将指导你完成在 Azure Application Insights 中收集和查看 `EventCounters`（系统定义的或用户定义的）所需的步骤。

## <a name="using-application-insights-to-collect-eventcounters"></a>使用 Application Insights 收集 EventCounters

Application Insights 支持使用 `EventCounterCollectionModule` 来收集 `EventCounters`，新发布的 nuget 包 [Microsoft.ApplicationInsights.EventCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventCounterCollector) 中包含该模块。 使用 [AspNetCore](asp-net-core.md) 或 [WorkerService](worker-service.md) 时，将自动启用 `EventCounterCollectionModule`。 `EventCounterCollectionModule` 以 60 秒的不可配置收集频率收集计数器。 收集 EventCounters 不需要特殊权限。

## <a name="default-counters-collected"></a>收集的默认计数器

对于在 .NET Core 3.0 中运行的应用，SDK 会自动收集以下计数器。 计数器的名称将采用“类别|计数器”的形式。

|Category | 计数器|
|---------------|-------|
|`System.Runtime` | `cpu-usage` |
|`System.Runtime` | `working-set` |
|`System.Runtime` | `gc-heap-size` |
|`System.Runtime` | `gen-0-gc-count` |
|`System.Runtime` | `gen-1-gc-count` |
|`System.Runtime` | `gen-2-gc-count` |
|`System.Runtime` | `time-in-gc` |
|`System.Runtime` | `gen-0-size` |
|`System.Runtime` | `gen-1-size` |
|`System.Runtime` | `gen-2-size` |
|`System.Runtime` | `loh-size` |
|`System.Runtime` | `alloc-rate` |
|`System.Runtime` | `assembly-count` |
|`System.Runtime` | `exception-count` |
|`System.Runtime` | `threadpool-thread-count` |
|`System.Runtime` | `monitor-lock-contention-count` |
|`System.Runtime` | `threadpool-queue-length` |
|`System.Runtime` | `threadpool-completed-items-count` |
|`System.Runtime` | `active-timer-count` |
|`Microsoft.AspNetCore.Hosting` | `requests-per-second` |
|`Microsoft.AspNetCore.Hosting` | `total-requests` |
|`Microsoft.AspNetCore.Hosting` | `current-requests` |
|`Microsoft.AspNetCore.Hosting` | `failed-requests` |

> [!NOTE]
> 仅在 ASP.NET Core 应用程序中添加 Microsoft.AspNetCore.Hosting 类别的计数器。

## <a name="customizing-counters-to-be-collected"></a>自定义要收集的计数器

下面的示例演示如何添加/删除计数器。 使用 `AddApplicationInsightsTelemetry()` 或 `AddApplicationInsightsWorkerService()` 启用 Application Insights 遥测收集后，将在应用程序的 `ConfigureServices` 方法中完成此自定义。 下面是 ASP.NET Core 应用程序中的示例代码。 有关其他类型的应用程序，请参阅[本文档](worker-service.md#configuring-or-removing-default-telemetrymodules)。

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        // The following code shows several customizations done to EventCounterCollectionModule.
        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // This removes all default counters.
                module.Counters.Clear();

                // This adds a user defined counter "MyCounter" from EventSource named "MyEventSource"
                module.Counters.Add(new EventCounterCollectionRequest("MyEventSource", "MyCounter"));

                // This adds the system counter "gen-0-size" from "System.Runtime"
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );

        // The following code removes EventCounterCollectionModule to disable the module completely.
        var eventCounterModule = services.FirstOrDefault<ServiceDescriptor>
                    (t => t.ImplementationType == typeof(EventCounterCollectionModule));
        if (eventCounterModule != null)
        {
            services.Remove(eventCounterModule);
        }
    }
```

## <a name="event-counters-in-metric-explorer"></a>指标资源管理器中的事件计数器

要在[指标资源管理器](/azure-monitor/platform/metrics-charts)查看 EventCounter 指标，请选择 Application Insights 资源并选择基于日志的指标作为指标命名空间。 然后，EventCounter 指标将显示在“自定义”类别下。

> [!div class="mx-imgBorder"]
> ![Application Insights 中报告的事件计数器](./media/event-counters/metrics-explorer-counter-list.png)

## <a name="event-counters-in-analytics"></a>Analytics 中的事件计数器

还可在 [Analytics](../../azure-monitor/log-query/log-query-overview.md) 下的 customMetrics 表中搜索和显示事件计数器报表。
<!-- Correct in MC: azure-monitor/log-query/log-query-overview.md -->

例如，运行以下查询，查看收集了哪些计数器并且可用于查询：

```Kusto
customMetrics | summarize avg(value) by name
```

> [!div class="mx-imgBorder"]
> ![Application Insights 中报告的事件计数器](./media/event-counters/analytics-event-counters.png)

要获取近期某个特定计数器的图表（例如：`ThreadPool Completed Work Item Count`），请运行以下查询。

```Kusto
customMetrics 
| where name contains "System.Runtime|ThreadPool Completed Work Item Count"
| where timestamp >= ago(1h)
| summarize  avg(value) by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```
> [!div class="mx-imgBorder"]
> ![Application Insights 中单个计数器的图表](./media/event-counters/analytics-completeditems-counters.png)

与其他遥测一样，customMetrics 同样也具有列 `cloud_RoleInstance`，指示正在运行应用的主机服务器实例的标识。 上面的查询显示每个实例的计数器值，并可用于比较不同服务器实例的性能。

## <a name="alerts"></a>警报
与其他指标一样，可以[设置警报](../../azure-monitor/app/alerts.md)，以便在事件计数器超出指定的限制时收到警报。 打开“警报”窗格，并单击“添加警报”。

## <a name="frequently-asked-questions"></a>常见问题

### <a name="can-i-see-eventcounters-in-live-metrics"></a>能否在实时指标中看到 EventCounters？

实时指标目前不显示 EventCounters。 请使用指标资源管理器或 Analytics 来查看遥测数据。

### <a name="which-platforms-can-i-see-the-default-list-of-net-core-30-counters"></a>可以在哪些平台上查看 .NET Core 3.0 计数器的默认列表？

EventCounter 无需任何特殊权限，所有受支持的 .NET Core 3.0 平台均支持 EventCounter。 这包括：

* **操作系统**：Windows、Linux 或 macOS。
* **托管方法**：进程内或进程外。
* **部署方法**：框架相关或独立。
* **Web 服务器**：IIS (Internet Information Server) 或 Kestrel。
* **托管平台**：Azure 应用服务的 Web 应用功能、Azure VM、Docker、Azure Kubernetes 服务 (AKS) 等。

### <a name="i-have-enabled-application-insights-from-azure-web-app-portal-but-i-cant-see-eventcounters"></a>我已在 Azure Web 应用门户中启用 Application Insights。 但为什么看不到 EventCounters？

 ASP.NET Core 的 [Application Insights 扩展](/azure-monitor/app/azure-web-apps)尚不支持此功能。 支持此功能后，本文档会更新。

## <a name="next-steps"></a><a name="next"></a>后续步骤

* [依赖关系跟踪](../../azure-monitor/app/asp-net-dependencies.md)

