---
title: Application Insights：语言、平台和集成 | Microsoft Docs
description: 适用于 Application Insights 的语言、平台和集成
ms.topic: conceptual
author: Johnnytechn
origin.date: 07/18/2019
ms.date: 05/28/2020
ms.reviewer: olegan
ms.author: v-johya
ms.openlocfilehash: 017e4ae6cc3fff46cb24b0fcbeb9317c1b17c5b4
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199789"
---
# <a name="supported-languages"></a>支持的语言

* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [JavaScript](../../azure-monitor/app/javascript.md)
* [Node.JS](../../azure-monitor/app/nodejs.md)
* [Python](../../azure-monitor/app/opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>支持的平台和框架

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>已部署应用程序的检测（无代码、基于代理）
* [Azure VM 和 Azure 虚拟机规模集](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [Azure 应用服务](../../azure-monitor/app/azure-web-apps.md)
* [ASP.NET - 适用于已处于活动状态的应用](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure 云服务](../../azure-monitor/app/cloudservices.md)，包括 Web 角色和辅助角色
### <a name="instrumentation-through-code-sdks"></a>通过代码进行检测 (SDK)
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Python](../../azure-monitor/app/opencensus-python.md)
* [Windows 桌面应用程序、服务和辅助角色](../../azure-monitor/app/windows-desktop.md)

## <a name="logging-frameworks"></a>记录框架
* [ILogger](/azure-monitor/app/ilogger)
* [Log4Net、NLog 或 System.Diagnostics.Trace](../../azure-monitor/app/asp-net-trace-logs.md)
* [Java、Log4J 或 Logback](../../azure-monitor/app/java-trace-logs.md)
* [LogStash 插件](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure Monitor](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="export-and-data-analysis"></a>导出和数据分析
* [Power BI](https://powerbi.microsoft.com/blog/explore-your-application-insights-data-with-power-bi/)
* [流分析](../../azure-monitor/app/export-power-bi.md)

## <a name="unsupported-sdks"></a>不受支持的 SDK
我们知道还有其他几个社区支持的 SDK。 但是，Azure Monitor 仅在使用此页上列出的受支持 SDK 时提供支持。 我们一直在评估扩展对其他语言支持的机会，因此请关注我们的 [GitHub 公告](https://github.com/microsoft/ApplicationInsights-Announcements/issues)页，以获得最新的 SDK 消息。 

