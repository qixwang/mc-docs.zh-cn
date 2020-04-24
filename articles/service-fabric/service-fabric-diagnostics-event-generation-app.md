---
title: Azure Service Fabric 应用程序级监视
description: 了解用于监视和诊断 Azure Service Fabric 群集的应用程序和服务级别的事件和日志。
author: rockboyfor
ms.topic: conceptual
origin.date: 11/21/2018
ms.date: 01/13/2020
ms.author: v-yeche
ms.openlocfilehash: c5ee92ff5691344e884f826fef43946c7083bc8f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75742314"
---
# <a name="application-logging"></a>应用程序日志记录

检测代码不仅是获取用户见解的一种方式，也是了解应用程序中是否存在问题以及诊断需要修复的内容的唯一方法。 尽管在技术上可将调试器连接到生产服务，但这种做法并不常见。 因此，提供详细的检测数据非常重要。

某些产品可自动检测代码。 尽管这些解决方案能够正常运行，但针对业务逻辑，几乎始终都要执行手动检测。 最后，必须提供足够的信息来对应用程序进行取证式的调试。 Service Fabric 应用程序可以使用任何记录框架进行检测。 本文将介绍检测代码的几种不同方法，以及如何在不同的方法之间做出选择。 

有关如何使用这些建议的示例，请参阅[向 Service Fabric 应用程序添加日志记录](service-fabric-how-to-diagnostics-log.md)。

<!--Not Available on ## Application Insights SDK-->
## <a name="eventsource"></a>EventSource

在 Visual Studio 中通过模板创建 Service Fabric 解决方案时，将生成 **EventSource** 派生类（**ServiceEventSource** 或 **ActorEventSource**）。 会创建一个模板，可将应用程序或服务的事件添加到其中。 **EventSource** 名称**必须**唯一，应该将它重命名，不要使用默认的模板字符串 MyCompany-&lt;solution&gt;-&lt;project&gt;。 使用多个同名的 **EventSource** 定义会导致运行时出现问题。 每个定义的事件必须具有唯一标识符。 如果标识符不唯一，将发生运行时失败。 某些组织为标识符预先分配了值范围，避免不同的开发团队之间发生冲突。 有关详细信息，请参阅 [Vance 的博客](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)或 [MSDN 文档](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx)。

## <a name="aspnet-core-logging"></a>ASP.NET Core 日志记录

必须认真规划如何检测代码。 适当的检测计划有助于避免代码基变得不稳定，从而需要重新检测代码。 为了降低风险，可以选择一个检测库，例如，Microsoft ASP.NET Core 中包含的 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/)。 ASP.NET Core 提供一个可在所选提供程序中使用的 [ILogger](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger) 接口，同时尽量减轻对现有代码的影响。 可以在 Windows 和 Linux 上的 ASP.NET Core 中以及整个 .NET Framework 中使用代码，从而将检测代码标准化。

<!--MOONCAKE: CORECT ON https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger-->

## <a name="next-steps"></a>后续步骤

选择了用于检测应用程序和服务的日志记录提供程序之后，需要聚合日志和事件才能将其发送到任何的分析平台。 阅读有关 [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) 的信息，更好地了解一些 Azure Monitor 推荐选项。

<!-- Not Available on [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)-->

<!--Update_Description: update meta properties, wording update-->