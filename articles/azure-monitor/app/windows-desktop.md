---
title: 监视 Windows 桌面应用的使用情况和性能
description: 使用 Application Insights 分析 Windows 桌面应用的使用情况和性能。
ms.topic: conceptual
author: Johnnytechn
origin.date: 10/29/2019
ms.date: 07/17/2020
ms.author: v-johya
ms.openlocfilehash: 8c2ad75776e53eeb92c713cb7d4bcabf8518ef58
ms.sourcegitcommit: 2b78a930265d5f0335a55f5d857643d265a0f3ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87244419"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>监视经典 Windows 桌面应用中的使用情况和性能

在 Azure 和其他云中本地托管的应用程序都可以利用 Application Insights。 唯一限制是需要[允许与 Application Insights 服务通信](../../azure-monitor/app/ip-addresses.md)。 若要监视通用 Windows 平台 (UWP) 应用程序，我们建议使用 [Visual Studio App Center](../../azure-monitor/learn/mobile-center-quickstart.md)。

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>将遥测从经典 Windows 应用程序发送到 Application Insights
1. 在 [Azure 门户](https://portal.azure.cn)中，[创建 Application Insights 资源](../../azure-monitor/app/create-new-resource.md )。 
2. 获取检测密钥的副本。
3. 在 Visual Studio 中，编辑应用项目的 NuGet 包，并添加 Microsoft.ApplicationInsights.WindowsServer。 （或者，如果只需要基本 API，而无需标准遥测收集模块，则选择 Microsoft.ApplicationInsights。）
4. 在代码中设置检测密钥：
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *你的密钥* `";`
   
    或在 ApplicationInsights.config 中设置检测密钥（如果已安装标准遥测包之一）：
   
    `<InstrumentationKey>`*你的密钥*`</InstrumentationKey>` 
   
    如果使用 ApplicationInsights.config，请确保它在解决方案资源管理器中的属性设置为“生成操作”=“内容”、“复制到输出目录”=“复制”。
5. [使用 API](../../azure-monitor/app/api-custom-events-metrics.md) 发送遥测。
6. 运行应用，并在 Azure 门户中创建的资源中查看遥测。

<a name="telemetry"></a>
## <a name="example-code"></a>示例代码
```csharp
using Microsoft.ApplicationInsights;

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
        {
            e.Cancel = true;

            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="override-storage-of-computer-name"></a>替代计算机名称的存储

默认情况下，此 SDK 将收集并存储发出遥测的系统的计算机名称。

计算机名由 Application Insights [旧的企业（按节点）定价层](/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier)用于内部计费。 默认情况下，如果使用遥测初始值设定项替代 `telemetry.Context.Cloud.RoleInstance`，则将发送一个单独的属性 `ai.internal.nodeName`，该属性仍将包含计算机名值。 此值不会与 Application Insights 遥测数据一起存储，而是在内部引入时使用，以允许向后兼容基于旧节点的计费模型。

如果你使用的是[旧的企业（按节点）定价层](/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier)，并且只需要替代计算机名的存储，请使用遥测初始值设定项：

**按如下所示编写自定义 TelemetryInitializer。**

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleInstance))
            {
                // Set custom role name here. Providing an empty string will result
                // in the computer name still be sent via this property.
                  telemetry.Context.Cloud.RoleInstance = "Custom RoleInstance";
            }
        }
    }
}
```

在以下设置检测密钥的 `Program.cs` `Main()` 方法中实例化初始化程序：

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

   static void Main()
        {
            TelemetryConfiguration.Active.InstrumentationKey = "{Instrumentation-key-here}";
            TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
            //...
        }
```

## <a name="override-transmission-of-computer-name"></a>替代计算机名的传输

如果你使用的不是[旧的企业（按节点）定价层](/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier)，并且想要完全阻止发送包含计算机名的任何遥测，则需要使用遥测处理器。

### <a name="telemetry-processor"></a>遥测处理器

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;


namespace WindowsFormsApp2
{
    public class CustomTelemetryProcessor : ITelemetryProcessor
    {
        private readonly ITelemetryProcessor _next;

        public CustomTelemetryProcessor(ITelemetryProcessor next)
        {
            _next = next;
        }

        public void Process(ITelemetry item)
        {
            if (item != null)
            {
                item.Context.Cloud.RoleInstance = string.Empty;
            }

            _next.Process(item);
        }
    }
}
```

在以下设置检测密钥的 `Program.cs` `Main()` 方法中实例化遥测处理器：

```csharp
using Microsoft.ApplicationInsights.Extensibility;

namespace WindowsFormsApp2
{
    static class Program
    {
        static void Main()
        {
            TelemetryConfiguration.Active.InstrumentationKey = "{Instrumentation-key-here}";
            var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
            builder.Use((next) => new CustomTelemetryProcessor(next));
            builder.Build();
            //...
        }
    }
}

```

> [!NOTE]
> 尽管技术上你可以使用如上所述的遥测处理器（即使你使用的是[旧的企业（按节点）定价层](/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier)），但这将由于无法针对每个节点定价区分节点而导致超额计费。

## <a name="next-steps"></a>后续步骤
* [创建仪表板](../../azure-monitor/app/overview-dashboard.md)
* [诊断搜索](../../azure-monitor/app/diagnostic-search.md)
* [探索指标](../../azure-monitor/platform/metrics-charts.md)
* [编写分析查询](../../azure-monitor/log-query/log-query-overview.md)
<!--Correct in MC: ../../azure-monitor/log-query/log-query-overview.md-->

