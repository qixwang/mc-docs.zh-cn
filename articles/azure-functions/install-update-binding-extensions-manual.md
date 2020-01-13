---
title: 手动安装或更新 Azure Functions 绑定扩展
description: 了解如何为已部署的函数应用安装或更新 Azure Functions 绑定扩展。
ms.topic: reference
ms.date: 12/31/2019
ms.openlocfilehash: ba7f54db2db3c3b981c77a88957f1e63b8264aeb
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75623657"
---
# <a name="manually-install-or-update-azure-functions-binding-extensions-from-the-portal"></a>从门户中手动安装或更新 Azure Functions 绑定扩展

从 2.x 版开始，Azure Functions 运行时使用绑定扩展实现触发器和绑定的代码。 NuGet 程序包中提供了绑定扩展。 注册某个扩展，实质上是安装某个程序包。 开发函数时，你安装绑定扩展的方式取决于开发环境。 有关详细信息，请参阅“触发器和绑定”一文中的[注册绑定扩展](./functions-bindings-register.md)。

有时候，你需要在 Azure 门户中手动安装或更新绑定扩展。 例如，可能需要将某个已注册的绑定更新为较新的版本。 你可能还需要注册无法在门户的“集成”选项卡中安装的受支持绑定。 

## <a name="install-a-binding-extension"></a>安装绑定扩展

使用以下步骤从门户中手动安装或更新扩展。

1. 在 [Azure 门户](https://portal.azure.cn)中，找到你的函数应用并选择它。 选择“概述”  选项卡并选择“停止”。   停止函数应用将解锁文件，以便可以进行更改。

1. 选择“平台功能”  选项卡，在“开发工具”  下选择“高级工具(Kudu)”。  Kudu 终结点 (`https://<APP_NAME>.scm.chinacloudsites.cn/`) 将在一个新窗口中打开。

1. 在 Kudu 窗口中，选择“调试控制台” > “CMD”。    

1. 在命令窗口中，导航到 `D:\home\site\wwwroot` 并选择 `bin` 旁边的删除图标以删除文件夹。 选择“确定”以确认删除  。

1. 选择 `extensions.csproj` 文件旁边的编辑按钮，该文件定义了函数应用的绑定扩展。 项目文件将从在线编辑器中打开。

1. 在 **ItemGroup** 中对 **PackageReference** 项进行必要的添加和更新，然后选择“保存”。  可以在 [What packages do I need?](https://github.com/Azure/azure-functions-host/wiki/Updating-your-function-app-extensions#what-nuget-packages-do-i-need)（我需要哪些程序包？）wiki 文章中找到受支持程序包版本的当前列表。 三个 Azure 存储绑定都需要 Microsoft.Azure.WebJobs.Extensions.Storage 程序包。

1. 从 `wwwroot` 文件夹中，运行以下命令以在 `bin` 文件夹中重新生成所引用的程序集。

    ```cmd
    dotnet build extensions.csproj -o bin --no-incremental --packages D:\home\.nuget
    ```

1. 返回到门户中的“概述”  选项卡，选择“启动”  以重启函数应用。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [详细了解 Azure Functions 触发器和绑定](functions-triggers-bindings.md)

<!-- Update_Description: wording update -->