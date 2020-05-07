---
title: include 文件
description: include 文件
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
origin.date: 04/15/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: f52fa57cc1693d70d431a9c4793f855368c5d656
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82595250"
---
1. 下载以下平台的客户端 SDK 快速入门：
    
    [iOS (Objective-C)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS)  
    [iOS (Swift)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS-Swift)  
    [Android (Java)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/android)  
    [Xamarin.iOS](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.iOS)  
    [Xamarin.Android](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.android)  
    [Xamarin.Forms](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.forms)  
    [Cordova](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/cordova)  
    [Windows (C#)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/windows-uwp-cs)  

    > [!NOTE]
    > 如果使用 iOS 项目，则需从[最新 GitHub 版本](https://github.com/Azure/azure-mobile-apps-ios-client/releases/latest)中下载“azuresdk-iOS-\*.zip”。 将 `MicrosoftAzureMobile.framework` 文件解压缩并添加到项目的根目录中。
    >

2. 需要添加数据库连接，或者连接到现有的连接。 首先，确定是要创建数据存储，还是使用现有的数据存储。

    - **创建新的数据存储**：若要创建数据存储，请使用以下快速入门：

        [快速入门：开始使用 Azure SQL 数据库中的单一数据库](/sql-database/sql-database-single-database-quickstart-guide)

    - **现有数据源**：若要使用现有的数据库连接，请按以下说明操作

        1. SQL 数据库连接字符串格式 - `Data Source=tcp:{your_SQLServer},{port};Initial Catalog={your_catalogue};User ID={your_username};Password={your_password}`

           **{your_SQLServer}** ：服务器的名称，此项可以在数据库的概览页中找到，通常采用“server_name.database.windows.net”格式。
            **{port}** ：通常为 1433。
            **{your_catalogue}** ：数据库的名称。
            **{your_username}** ：用于访问数据库的用户名。
            **{your_password}** ：用于访问数据库的密码。

            [详细了解 SQL 连接字符串格式](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-string-syntax#sqlclient-connection-strings)

        2. 向**移动应用**添加连接字符串。可以在应用服务中管理应用程序的连接字符串，方法是使用菜单中的“配置”选项。 

            若要添加连接字符串，请执行以下操作：

            1. 单击“应用程序设置”  选项卡。

            2. 单击“[+] 新建连接字符串”。 

            3. 需为连接字符串提供“名称”、“值”和“类型”。   

            4. 键入 `MS_TableConnectionString` 作为“名称” 

            5. “值”应该是在前面的步骤中生成的连接字符串。

            6. 如果向 SQL Azure 数据库添加连接字符串，请在“类型”下选择“SQLAzure”。  

3. Azure 移动应用有适用于 .NET 和 Node.js 后端的 SDK。

   - **Node.js backend**
    
     若要使用 Node.js 快速入门应用，请按以下说明操作。
     
        1. 创建新 API - 可以直接在 Azure 门户中进行更改，也可以在开发环境中以本地方式修改代码，然后将其发布到 Azure。 单击“`Development Tools`”菜单下的“`App Service Editor (Preview)`”即可获得应用代码的浏览器内编辑体验。
        
        2. 单击“`Go`”，待应用服务编辑器打开后，就可以完全控制源代码了。 假设已使用 npm install 命令安装了 express 和 azure-mobile-apps 包，请单击 WWWROOT 下的 api 文件夹以创建或编辑自定义 API。 对代码文件进行更改，所做的更改会自动保存。
        
        3. 可以完全控制用于存储应用程序数据的 Azure SQL 数据库。 可以轻松地在数据库中创建新表。
 
   - **.NET 后端**
    
        若要使用 .NET 快速入门应用，请按以下说明操作。

        1. 从 [azure-mobile-apps-quickstarts 存储库](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/dotnet/Quickstart)下载 Azure 移动应用 .NET 服务器项目。

        2. 在 Visual Studio 中以本地方式生成 .NET 服务器项目。

        3. 在 Visual Studio 中打开解决方案资源管理器，右键单击 `ZUMOAPPNAMEService` 项目，单击“发布”，  此时会出现“`Publish to App Service`”窗口。 如果在 Mac 上工作，请单击[此处](https://docs.azure.cn/app-service/deploy-local-git)了解部署应用的其他方式。
        
           ![Visual Studio 发布](./media/app-service-mobile-configure-new-backend/visual-studio-publish.png)

        4. 选择“应用服务”  作为发布目标，接着单击“选择现有”，然后单击窗口底部的“发布”按钮。  

        5. 首先需通过 Azure 订阅登录 Visual Studio。 选择 `Subscription`、`Resource Group`，然后选择应用的名称。 准备就绪后，单击“确定”，这样就会将本地的 .NET 服务器项目部署到应用服务后端。  部署完成后，系统会在浏览器中将你重定向到 `http://{zumoappname}.chinacloudsites.cn/`。
