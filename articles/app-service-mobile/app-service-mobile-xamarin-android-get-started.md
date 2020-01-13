---
title: Xamarin.Android 应用入门
description: 按照本教程进行操作，开始使用 Azure 移动应用进行 Xamarin Android 开发。
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: conceptual
origin.date: 06/25/2019
ms.date: 12/16/2019
ms.author: v-tawe
ms.openlocfilehash: 762d2cbdbe8aa9423469123765ac18b5902fd986
ms.sourcegitcommit: cebee33429c25996658d322d337dd05ad1439f89
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75600539"
---
# <a name="create-a-xamarinandroid-app"></a>创建 Xamarin.Android 应用
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center 支持以移动应用开发为中心的端到端集成服务。 开发人员可以使用“生成”  、“测试”  和“分发”  服务来设置“持续集成和交付”管道。 部署应用后，开发人员可以使用**分析**和**诊断**服务监视其应用的状态和使用情况，并使用**推送**服务与用户互动。 开发人员还可以利用“身份验证”  对其用户进行身份验证，并使用“数据”  服务在云中保留和同步应用数据。
>
> 如果希望将云服务集成到移动应用程序中，请立即注册到 [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) 中。

## <a name="overview"></a>概述
本教程说明如何向 Xamarin.Android 应用添加基于云的后端服务。 有关详细信息，请参阅 [什么是移动应用](app-service-mobile-value-prop.md)。

以下是完成的应用程序的屏幕快照：

![][0]

只有在完成本教程后，才可以学习有关 Xamarin Android 应用的所有其他移动应用教程。

## <a name="prerequisites"></a>先决条件
若要完成本教程，需要满足以下先决条件：

* 有效的 Azure 帐户。 如果没有帐户，可以注册 Azure 试用版并获取多达 10 个免费的移动应用。 有关详细信息，请参阅 [Azure 1 元试用](https://www.azure.cn/pricing/1rmb-trial/)。
* Visual Studio with Xamarin。 有关说明，请参阅 [设置和安装 Visual Studio 和 Xamarin](https://docs.microsoft.com/visualstudio/cross-platform/setup-and-install) 。

## <a name="create-an-azure-mobile-app-backend"></a>创建 Azure 移动应用后端
按照下列步骤创建移动应用后端。

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

现已预配可供移动客户端应用程序使用的 Azure 移动应用后端。 接下来，为简单的“待办事项列表”后端下载服务器项目并将其发布到 Azure。

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>创建数据库连接并配置客户端和服务器项目
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinandroid-app"></a>运行 Xamarin.Android 应用
1. 打开 Xamarin.Android 项目。

2. 转到 [Azure 门户](https://portal.azure.cn/)，并导航到已创建的移动应用。 在 `Overview` 边栏选项卡上，查找作为移动应用公共终结点的 URL。 示例 - 我的应用名称“test123”的站点名将为 https://test123.chinacloudsites.cn 。

3. 打开此文件夹中的文件 `ToDoActivity.cs` - xamarin.android/ZUMOAPPNAME/ToDoActivity.cs。 应用程序名称为 `ZUMOAPPNAME`。

4. 在 `ToDoActivity` 类中，将 `ZUMOAPPURL` 变量替换为上面的公共终结点。

    `const string applicationURL = @"ZUMOAPPURL";`

    变为
    
    `const string applicationURL = @"https://test123.chinacloudsites.cn";`
    
5. 按 F5 键部署并运行应用。

6. 在应用中键入有意义的文本（例如“完成教程”  ），然后单击“添加”  按钮。

    ![][10]

    来自请求的数据插入到 TodoItem 表。 移动应用后端返回存储在表中的项，数据显示在列表中。

   > [!NOTE]
   > 可以查看访问移动应用后端以查询和插入数据的代码，这些代码在 ToDoActivity.cs C# 文件中。
   
## <a name="troubleshooting"></a>故障排除
如果在生成解决方案时遇到问题，请运行 NuGet 包管理器并更新 `Xamarin.Android` 支持包。 快速入门项目并非始终包含最新版本。

请注意，在项目中引用的所有支持包必须都具有相同的版本。 对于 Android 平台，[Azure 移动应用 NuGet 包](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)具有 `Xamarin.Android.Support.CustomTabs` 依赖项，因此，如果你的项目使用较新的支持包，则你需要直接安装具有所需版本的此包以避免冲突。

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png
<!-- URLs. -->
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203