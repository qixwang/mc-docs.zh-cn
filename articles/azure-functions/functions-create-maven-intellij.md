---
title: 使用 Java 和 IntelliJ 创建 Azure 函数
description: 了解如何使用 Java 和 IntelliJ 在 Azure 上创建和发布简单的 HTTP 触发式无服务器应用。
author: jeffhollan
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: v-junlch
ms.custom: mvc, devcenter
ms.openlocfilehash: 883173c8b818de149c4e9cd9f76617231a31664c
ms.sourcegitcommit: e500354e2fd8b7ac3dddfae0c825cc543080f476
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2020
ms.locfileid: "79546874"
---
# <a name="create-your-first-azure-function-with-java-and-intellij"></a>使用 Java 和 IntelliJ 创建第一个 Azure 函数

本文介绍：
- 如何使用 IntelliJ IDEA 和 Apache Maven 创建[无服务器](https://azure.microsoft.com/overview/serverless-computing/)函数项目
- 在自己的计算机上的集成开发环境 (IDE) 中测试和调试函数的步骤
- 将函数项目部署到 Azure Functions 的说明

<!-- TODO ![Access a Hello World function from the command line with cURL](./media/functions-create-java-maven/hello-azure.png) -->

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="set-up-your-development-environment"></a>设置开发环境

若要使用 Java 和 IntelliJ 开发函数，请安装以下软件：

- [Java 开发人员工具包](https://www.azul.com/downloads/zulu/) (JDK) 版本 8
- [Apache Maven](https://maven.apache.org) 3.0 或更高版本
- 带 Maven 的 [IntelliJ IDEA](https://www.jetbrains.com/idea/download) 社区版或旗舰版
- [Azure CLI](/cli)

> [!IMPORTANT]
> JAVA_HOME 环境变量必须设置为 JDK 的安装位置才能完成本文中的步骤。

 建议安装 [Azure Functions Core Tools 版本 2](functions-run-local.md#v2)。 它为编写、运行和调试 Azure Functions 提供了本地开发环境。

## <a name="create-a-functions-project"></a>创建 Functions 项目

1. 在 IntelliJ IDEA 中选择“Create New Project”（创建新项目）  。  
1. 在“New Project”（新建项目）窗口的左窗格中，选择“Maven”。  
1. 为 [azure-functions-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype) 选中“Create from archetype”（通过 archetype 创建）复选框，然后选中“Add Archetype”（添加 Archetype）的复选框   。
1. 在“Add Archetype”（添加 Archetype）窗口中完成相关字段，如下所示： 
    - _GroupId_：com.microsoft.azure
    - _ArtifactId_：azure-functions-archetype
    - _版本_：检查和使用[中央存储库](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)中的最新版本
    ![在 IntelliJ IDEA 中基于原型创建 Maven 项目](./media/functions-create-first-java-intellij/functions-create-intellij.png)  
1. 选择“确定”  ，然后选择“下一步”  。
1. 输入当前项目的详细信息，然后选择“Finish”（完成）  。

Maven 在新文件夹中项目文件，该文件夹使用与 _ArtifactId_ 值相同的名称。 项目的生成代码是一个简单的 [HTTP 触发的](/azure-functions/functions-bindings-http-webhook)函数，回显触发 HTTP 请求的正文。

## <a name="run-functions-locally-in-the-ide"></a>在 IDE 本地运行函数

> [!NOTE]
> 若要在本地运行和调试函数，请确保已安装 [Azure Functions Core Tools 版本 2](functions-run-local.md#v2)。

1. 手动导入所做的更改，或者启用[自动导入](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html)。
1. 打开“Maven Projects”（Maven 项目）工具栏。 
1. 展开“Lifecycle”（生命周期），  然后打开“package”（包）  。 此时会在新创建的目标目录中生成解决方案并将其打包。
1. 展开“Plugins”（插件）   >   “azure-functions”，打开 **azure-functions:run**，以便启动 Azure Functions 本地运行时。  
  ![Azure Functions 的 Maven 工具栏](./media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

1. 完成函数测试后关闭运行对话框。 一次只能有一个函数主机处于活动状态并在本地运行。

## <a name="debug-the-function-in-intellij"></a>在 IntelliJ 中调试函数

1. 若要在调试模式下启动函数主机，请在运行函数时添加 **-DenableDebug** 作为参数。 可以在 [maven 目标](https://www.jetbrains.com/help/idea/maven-support.html#run_goal)中更改配置，或在终端窗口中运行以下命令：  

   ```
   mvn azure-functions:run -DenableDebug
   ```

   该命令导致函数主机打开调试端口 5005。

1. 在“Run”（运行）  菜单中选择“Edit Configurations”（编辑配置）。 
1. 选择“(+)”，添加“Remote”（远程）。  
1. 完成“Name”（名称）和“Settings”（设置）字段   ，然后选择“OK”（确定）  以保存配置。
1. 在设置后，选择“Debug”（调试）>“Remote Configuration Name”（远程配置名称）或在键盘上按 Shift+F9 以启动调试。 

1. 完成后，停止调试器和正在运行的进程。 一次只能有一个函数主机处于活动状态并在本地运行。

## <a name="deploy-the-function-to-azure"></a>将函数部署到 Azure

1. 在向 Azure 部署函数之前，必须[使用 Azure CLI 登录](/cli/authenticate-azure-cli?view=azure-cli-latest)。

   ``` azurecli
   az login
   ```

1. 使用 `azure-functions:deploy` Maven 目标将代码部署到新的函数中。 也可在“Maven Projects”（Maven 项目）窗口中选择“azure-functions:deploy”选项。 

   ```
   mvn azure-functions:deploy
   ```

1. 成功部署函数后，请在 Azure CLI 输出中找到函数的 URL。

   ``` output
   [INFO] Successfully deployed Function App with package.
   [INFO] Deleting deployment package from Azure Storage...
   [INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
   [INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.chinacloudsites.cn
   [INFO] ------------------------------------------------------------------------
   ```

## <a name="next-steps"></a>后续步骤

- 有关开发 Java 函数的详细信息，请查看 [Java 函数开发人员指南](functions-reference-java.md)。
- 使用 `azure-functions:add` Maven 目标将具有不同触发器的其他函数添加到项目。

<!-- Update_Description: wording update -->