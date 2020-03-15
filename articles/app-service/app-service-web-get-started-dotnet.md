---
title: 快速入门：创建 C# ASP.NET Core 应用
description: 了解如何通过部署 Visual Studio 提供的默认 C# ASP.NET Core Web 应用模板，在 Azure 应用服务中运行 Web 应用。
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.topic: quickstart
origin.date: 08/30/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.custom: mvc, devcenter, vs-azure
ms.openlocfilehash: 3b2496278ec11466926447afefeccfcc6f3ba832
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79292676"
---
# <a name="create-an-aspnet-core-web-app-in-azure"></a>在 Azure 中创建 ASP.NET Core Web 应用

<!-- > [!NOTE] -->
<!-- > This article deploys an app to App Service on Windows. To deploy to App Service on _Linux_, see [Create a .NET Core web app in App Service on Linux](./containers/quickstart-dotnetcore.md). -->

[Azure 应用服务](overview.md)提供高度可缩放、自修复的 Web 托管服务。

本快速入门演示如何将第一个 ASP.NET Core Web 应用部署到 Azure 应用服务中。 完成后，将拥有一个资源组，该资源组包含一个应用服务计划和一个部署了 Web 应用程序的应用服务应用。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

为完成此教程，请安装支持 **ASP.NET 和 Web 开发**工作负荷的 <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a>。

如果已安装 Visual Studio 2019：

- 通过选择“帮助”   >   “检查更新”，在 Visual Studio 中安装最新更新。
- 通过选择“工具”   >   “获取工具和功能”，添加工作负荷。

## <a name="create-an-aspnet-core-web-app"></a>创建一个 ASP.NET Core Web 应用

按照以下步骤创建 ASP.NET Core Web 应用：

1. 打开 Visual Studio，然后选择“创建新项目”  。

1. 在“创建新项目”  中，找到并选择用于 C# 的 **ASP.NET Core Web 应用程序**，然后选择“下一步”  。

1. 在“配置新项目”  中，将应用程序命名为“myFirstAzureWebApp”  ，然后选择“创建”  。

   ![配置 Web 应用项目](./media/app-service-web-get-started-dotnet/configure-web-app-project.png)

1. 对于此快速入门，请选择“Web 应用程序”  模板。 确保身份验证设置为“无身份验证”  ，并且未选择其他选项。 选择“创建”  。

   ![对于本教程，请选择“ASP.NET Core Razor Pages”](./media/app-service-web-get-started-dotnet/aspnet-razor-pages-app.png)

    可将任何类型的 ASP.NET Core Web 应用部署到 Azure。

1. 在 Visual Studio 菜单中，选择“调试” > “开始执行(不调试)”以在本地运行 Web 应用。  

   ![在本地运行应用](./media/app-service-web-get-started-dotnet/razor-web-app-running-locally.png)

## <a name="publish-your-web-app"></a>发布 Web 应用

1. 在“解决方案资源管理器”  中右键单击“myFirstAzureWebApp”  项目，然后选择“发布”  。

1. 选择“应用服务”  ，然后选择“发布”  。

   ![从项目概述页发布](./media/app-service-web-get-started-dotnet/publish-app-vs2019.png)

1. 在“创建新的应用服务”  中，你的选项取决于你是否已登录 Azure 以及是否有一个链接到 Azure 帐户的 Visual Studio 帐户。 选择“添加帐户”  或“登录”  以登录 Azure 订阅。 如果你已经登录，请选择所需的帐户。

   > [!NOTE]
   > 如果已经登录，请先不要选择“创建”  。
   >

   ![登录 Azure](./media/app-service-web-get-started-dotnet/sign-in-azure-vs2019.png)

   [!INCLUDE [resource group intro text](../../includes/resource-group.md)]

1. 对于“资源组”  ，选择“新建”  。

1. 在“新资源组名称”  中，输入“myResourceGroup”  并选择“确定”  。

   [!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

1. 对于**托管计划**，请选择“新建”  。

1. 在“配置托管计划”  对话框中，输入下表中的值，然后选择“确定”  。

   | 设置 | 建议的值 | 说明 |
   |-|-|-|
   |应用服务计划| myAppServicePlan | 应用服务计划的名称。 |
   | 位置 | 中国东部 | 托管 Web 应用的数据中心。 |
   | 大小 | 免费 | [定价层](https://www.azure.cn/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)确定托管功能。 |

   ![创建应用服务计划](./media/app-service-web-get-started-dotnet/app-service-plan-vs2019.png)

1. 在**名称**中，输入仅包含有效字符 `a-z`、`A-Z`、`0-9` 和 `-` 的唯一应用名称。 可以接受自动生成的唯一名称。 Web 应用的 URL 为 `http://<app_name>.chinacloudsites.cn`，其中 `<app_name>` 是应用名称。

   ![配置应用名称](./media/app-service-web-get-started-dotnet/web-app-name-vs2019.png)

1. 选择“创建”  开始创建 Azure 资源。

向导完成后，它会将 ASP.NET Core Web 应用发布到 Azure，然后在默认浏览器中启动该应用。

![已在 Azure 中发布的 ASP.NET Web 应用](./media/app-service-web-get-started-dotnet/web-app-running-live.png)

在“创建新的应用服务”  页中指定的应用名称将用作 `http://<app_name>.chinacloudsites.cn` 格式的 URL 前缀。

**祝贺你！** 你的 ASP.NET Core Web 应用已在 Azure 应用服务中实时运行！

## <a name="update-the-app-and-redeploy"></a>更新应用并重新部署

1. 在**解决方案资源管理器**中你的项目下，打开  “页” > “Index.cshtml”  。

1. 将两个 `<div>` 标记替换为以下代码：

   ```HTML
   <div class="jumbotron">
       <h1>ASP.NET in Azure!</h1>
       <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
   </div>
   ```

1. 若要重新部署到 Azure，请在“解决方案资源管理器”  中右键单击“myFirstAzureWebApp”  项目，然后选择“发布”  。

1. 在“发布”摘要页中选择“发布”   。

   ![Visual Studio“发布”摘要页](./media/app-service-web-get-started-dotnet/publish-summary-page-vs2019.png)

发布完成后，Visual Studio 将启动浏览器并转到 Web 应用的 URL。

![已在 Azure 中更新的 ASP.NET Web 应用](./media/app-service-web-get-started-dotnet/web-app-running-live-updated.png)

## <a name="manage-the-azure-app"></a>管理 Azure 应用

若要管理 Web 应用，请转到 [Azure 门户](https://portal.azure.cn)，搜索并选择“应用服务”  。

![选择应用服务](./media/app-service-web-get-started-dotnet/app-services.png)

在“应用服务”页上，选择 Web 应用的名称  。

![在门户中导航到 Azure 应用](./media/app-service-web-get-started-dotnet/access-portal-vs2019.png)

随后会显示 Web 应用的概述页。 在这里，可以进行基本的管理，如浏览、停止、启动、重启和删除。

![Azure 门户中的应用服务](./media/app-service-web-get-started-dotnet/web-app-general-vs2019.png)

左侧菜单提供用于配置应用的不同页面。

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [将 ASP.NET Core 与 SQL 数据库配合使用](app-service-web-tutorial-dotnetcore-sqldb.md)
