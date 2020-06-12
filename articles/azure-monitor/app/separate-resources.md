---
title: 如何设计 Application Insights 部署- 一个资源与多个资源？
description: 为开发、测试和生产戳记直接遥测不同的资源。
ms.topic: conceptual
author: Johnnytechn
origin.date: 05/15/2017
ms.date: 05/28/2020
ms.author: v-johya
ms.openlocfilehash: f16489f329374df1ddd871c3dc8b14c10dde0dba
ms.sourcegitcommit: 5ae04a3b8e025986a3a257a6ed251b575dbf60a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2020
ms.locfileid: "84440730"
---
# <a name="how-many-application-insights-resources-should-i-deploy"></a>应该部署多少个 Application Insights 资源

部署 Web 应用程序的下一个版本时，不希望将新版本和已发布的版本中的 [Application Insights](../../azure-monitor/app/app-insights-overview.md) 遥测混合使用。 为避免混淆，请使用不同的检测密钥 (ikey) 将遥测数据从不同的开发阶段发送到不同的 Application Insights 资源。 为了在版本从一个阶段移动到另一个阶段时更轻松地更改检测密钥，在代码中而非在配置文件中设置 ikey 可能比较有用。

（如果系统是 Azure 云服务，有[另一种方法可以设置单独 ikey](../../azure-monitor/app/cloudservices.md)。）

## <a name="about-resources-and-instrumentation-keys"></a>关于资源和检测密钥

为 Web 应用设置 Application Insights 监视时，会在 Azure 中创建 Application Insights 资源。 为了查看和分析从应用收集的遥测数据，会在 Azure 门户中打开此资源。 每个资源都由一个*检测密钥* (iKey) 予以标识。 在安装 Application Insights 程序包来监视应用时，将为其配置检测密钥，以使其知道要将遥测数据发送到何处。

每个 Application Insights 资源随附了现成可用的指标。 如果完全独立的组件向相同的 Application Insights 资源报告，这些指标对仪表板/警报操作可能没有太大作用。

### <a name="when-to-use-a-single-application-insights-resource"></a>何时使用单个 Application Insights 资源

-   适合需要一起部署的应用程序组件。 通常由单个团队开发，由同一组 DevOps/ITOps 用户管理。
-   如果在默认情况下有必要对所有组件的关键绩效指标 (KPI) 进行聚合，如响应持续时间、仪表板中的故障率等（可以在指标资源管理器体验中选择按角色名称划分）。
-   如果不需要在应用程序组件之间以不同方式管理基于角色的访问控制 (RBAC)。
-   如果不需要为不同组件设置不同指标警报条件。
-   如果不需要以不同方式管理不同组件的连续导出。
-   如果不需要以不同方式管理不同组件的计费/配额。
-   如果可以让一个 API 密钥对所有组件中的数据具有相同的访问权限。 且设置 10 个 API 密钥就足以满足所有组件的相关需求。
-   如果可以对所有角色使用相同的智能检测和工作项集成设置。

### <a name="other-things-to-keep-in-mind"></a>需要记住的其他要点

-   你可能需要添加自定义代码，以确保在 [Cloud_RoleName](/azure-monitor/app/app-map?tabs=net#set-cloud-role-name) 属性中设置了有意义的值。 如果没有为此属性设置有意义的值，门户中的任何体验都不起作用。
- 对于 Service Fabric 应用程序和经典云服务，SDK 会自动从 Azure 角色环境中读取并设置上述内容。 对于所有其他类型的应用，可能需要显式设置这些内容。
-   实时指标体验不支持按角色名称拆分。

## <a name="dynamic-instrumentation-key"></a><a name="dynamic-ikey"></a> 动态检测密钥

为了在代码在不同生产阶段中移动时更轻松地更改 ikey，请在代码中而非在配置文件中设置 ikey。

在初始化方法中设置密钥，如 ASP.NET 服务中的 global.aspx.cs：

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

在此示例中，不同资源的 ikey 放置在 Web 配置文件的不同版本中。 通过交换 Web 配置文件（可作为发布脚本的一部分执行），将交换目标资源。

### <a name="web-pages"></a>网页
应用的网页中也使用 iKey，在[从快速启动窗格获取的脚本](../../azure-monitor/app/javascript.md)中使用。 从服务器状态生成它，而不是逐字将其编码到脚本中。 例如，在 ASP.NET 应用中：

*使用 Razor 的 JavaScript*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>创建其他 Application Insights 资源

若要创建 Application Insights 资源，请遵循[资源创建指南](/azure-monitor/app/create-new-resource)。

### <a name="getting-the-instrumentation-key"></a>获取检测密钥
检测密钥标识所创建的资源。

需要将向其发送数据的所有资源的检测密钥。

## <a name="filter-on-build-number"></a>按版本号筛选
发布新应用版本时，我们希望能够将不同版本的遥测数据分开。

可以设置“应用程序版本”属性，这样便可以筛选[搜索](../../azure-monitor/app/diagnostic-search.md)和[指标资源管理器](../../azure-monitor/platform/metrics-charts.md)结果。

可通过多种不同的方法设置“应用程序版本”属性。

* 直接设置：

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* 在[遥测初始值设定项](../../azure-monitor/app/api-custom-events-metrics.md#defaults)中将该行换行，确保以一致的方式设置所有 TelemetryClient 实例。
* [ASP.NET] 在 `BuildInfo.config` 中设置版本。 Web 模块将从 BuildLabel 节点选择版本。 在项目中包含此文件，并记得在解决方案资源管理器中设置“始终复制”属性。

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] 在 MSBuild 中自动生成 BuildInfo.config。 为此，请在 `.csproj` 文件中添加以下几行：

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    这会生成一个名为 *yourProjectName*.BuildInfo.config 的文件。发布过程会将此文件重命名为 BuildInfo.config。

    当使用 Visual Studio 生成时，生成标签包含一个占位符 (AutoGen_...)。 但是，在使用 MSBuild 生成时，标签中会填充正确的版本号。

    若要允许 MSBuild 生成版本号，请在 AssemblyReference.cs 中设置类似于 `1.0.*` 的版本

## <a name="version-and-release-tracking"></a>版本和发行版本跟踪
若要跟踪应用程序版本，请确保 Azure 生成引擎进程生成了 `buildinfo.config`。 在 .csproj 文件中，添加：  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

当它具有内部信息时，Application Insights Web 模块自动将**应用程序版本**作为属性添加到每个遥测项。 这样，便可以在执行[诊断搜索](../../azure-monitor/app/diagnostic-search.md)或[浏览指标](../../azure-monitor/platform/metrics-charts.md)时按版本进行筛选。

但请注意，内部版本号只能由 Azure 生成引擎生成，而不能由 Visual Studio 中的开发人员生成引擎生成。

## <a name="next-steps"></a>后续步骤

* [多个角色的共享资源](../../azure-monitor/app/app-map.md)
* [创建遥测初始值设定项来区分 A|B 变体](../../azure-monitor/app/api-filtering-sampling.md#add-properties)

