---
title: 教程 - 使用 Azure IoT Edge 开发用于 Windows 设备的模块
description: 本教程逐步介绍如何设置开发计算机和云资源，以使用适用于 Windows 设备的 Windows 容器开发 IoT Edge 模块
author: kgremban
manager: philmea
ms.author: v-tawe
origin.date: 11/11/2019
ms.date: 03/02/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: f6252f895e45ac0686942c7d21523e73eda7a9d3
ms.sourcegitcommit: f5bc5bf51a4ba589c94c390716fc5761024ff353
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77494531"
---
# <a name="tutorial-develop-iot-edge-modules-for-windows-devices"></a>教程：开发适用于 Windows 设备的 IoT Edge 模块

使用 Visual Studio 开发代码并将其部署到运行 IoT Edge 的 Windows 设备。

在快速入门中，你已使用 Windows 虚拟机创建了一个 IoT Edge 设备，并通过 Azure 市场部署了一个预生成的模块。 本教程将逐步介绍如何开发你自己的代码并将其部署到 IoT Edge 设备。 本教程是学习其他教程的有用先决条件，其他教程将更详细地介绍特定编程语言或 Azure 服务。

本教程使用**将 C# 模块部署到 Windows 设备**的示例。 之所以选择了此示例，原因在于它是最常见的开发方案。 如果你希望使用不同的语言进行开发，或者计划将 Azure 服务作为模块来部署，本教程还有助于了解开发工具。 了解开发概念之后，可以选择首选的语言或 Azure 服务深入到详细信息。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
>
> * 设置开发计算机。
> * 使用适用于 Visual Studio 的 IoT Edge Tools 创建新项目。
> * 将项目生成为容器，并将其存储在 Azure 容器注册表中。
> * 将代码部署到 IoT Edge 设备。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="key-concepts"></a>关键概念

本教程将逐步讲解 IoT Edge 模块的开发。 IoT Edge 模块（有时简称为“模块”）是包含可执行代码的容器。   可将一个或多个模块部署到 IoT Edge 设备。 模块执行特定的任务，例如，从传感器引入数据、执行数据分析或数据清理操作，或者将消息发送到 IoT 中心。 有关详细信息，请参阅[了解 Azure IoT Edge 模块](iot-edge-modules.md)。

开发 IoT Edge 模块时，必须了解开发计算机与该模块最终要部署到的目标 IoT Edge 设备之间的差异。 生成的用于保存模块代码的容器必须与目标设备的操作系统 (OS) 相匹配。  对于 Windows 容器开发，此概念更为简单，因为 Windows 容器仅在 Windows 操作系统上运行。 但是，举例而言，可以使用 Windows 开发计算机来生成适用于 Linux IoT Edge 设备的模块。 在这种情况下，必须确保开发计算机运行的是 Linux 容器。 在学习本教程的过程中，请注意开发计算机 OS 与容器 OS 之间的差异。  

本教程面向运行 IoT Edge 的 Windows 设备。 Windows IoT Edge 设备使用 Windows 容器。 我们建议使用 Visual Studio 进行 Windows 设备开发，这也是本教程将使用的工具。 也可以使用 Visual Studio Code，不过，这两个工具之间存在支持方面的差异。

下表列出 Visual Studio Code 和 Visual Studio 支持的 **Windows 容器**开发方案。

|   | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Azure 服务** | Azure Functions <br> Azure 流分析 |   |
| **语言** | C#（不支持调试） | C <br> C# |
| **详细信息** | [适用于 Visual Studio Code 的 Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [适用于 Visual Studio 2017 的 Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools)<br>[适用于 Visual Studio 2019 的 Azure IoT Edge 工具](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) |

## <a name="prerequisites"></a>必备条件

一台开发计算机：

* 装有 1809 更新或更高版本的 Windows 10。
* 可以根据开发偏好，使用自己的计算机或虚拟机。
  * 请确保开发计算机支持嵌套虚拟化。 此功能对于运行容器引擎是必需的，你将在下一部分中安装。
* 安装 [Git](https://git-scm.com/)。

Windows 上的一个 Azure IoT Edge 设备：

* 我们建议不要在开发计算机上运行 IoT Edge，而是使用独立的设备。 分开使用开发计算机和 IoT Edge 设备可以更准确地反映真实的部署方案，并且有助于直接体现不同的概念。
* 如果没有另一个可用的设备，请参考快速入门文章使用 [Windows 虚拟机](quickstart.md)在 Azure 中创建一个 IoT Edge 设备。

云资源：

* Azure 中的免费或标准层 [IoT 中心](../iot-hub/iot-hub-create-through-portal.md)。

## <a name="install-container-engine"></a>安装容器引擎

IoT Edge 模块打包为容器，因此，需要在开发计算机上安装一个容器引擎用于生成和管理容器。 我们建议使用 Docker Desktop 进行开发，由于它包含许多的功能，是非常流行的容器引擎。 在 Windows 计算机上使用 Docker Desktop 可以在 Linux 容器与 Windows 容器之间切换，以便轻松地为不同类型的 IoT Edge 设备开发模块。

参考 Docker 文档在开发计算机上安装：

* [安装适用于 Windows 的 Docker Desktop](https://docs.docker.com/docker-for-windows/install/)

  * 安装适用于 Windows 的 Docker Desktop 时，系统会询问你是要使用 Linux 还是 Windows 容器。 本教程使用 **Windows 容器**。 有关详细信息，请参阅[在 Windows 与 Linux 容器之间切换](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)。

## <a name="set-up-visual-studio-and-tools"></a>安装 Visual Studio 和工具

适用于 Visual Studio 的 IoT 扩展有助于开发 IoT Edge 模块。 这些扩展提供项目模板、自动创建部署清单，并可让你监视和管理 IoT Edge 设备。 在本部分，你将安装 Visual Studio 和 IoT Edge 扩展，然后设置 Azure 帐户以从 Visual Studio 内部管理 IoT 中心资源。

本教程讲解适用于 Visual Studio 2019 的开发步骤。 如果使用 Visual Studio 2017（15.7 或更高版本），则步骤相似。 如果你偏向于使用 Visual Studio Code，请参阅[使用 Visual Studio Code 开发和调试 Azure IoT Edge 的模块](how-to-vs-code-develop-module.md)中的说明。

1. 在开发计算机上准备 Visual Studio 2019。

   * 如果开发计算机上尚未安装 Visual Studio，请[安装 Visual Studio 2019](https://docs.microsoft.com/visualstudio/install/install-visual-studio) 及以下工作负荷：

      * Azure 开发
      * 使用 C++ 的桌面开发
      * .NET Core 跨平台开发

   * 如果开发计算机上确实已有 Visual Studio 2019，请按照[修改 Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) 中的步骤添加所需的工作负荷。

2. 下载并安装适用于 Visual Studio 2019 的 [Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) 扩展。

   如果使用的是 Visual Studio 2017（15.7 或更高版本），请下载并安装[适用于 Visual Studio 2017 的 Azure IoT Edge 工具](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools)。

3. 安装完成后，打开 Visual Studio 2019 并选择“在没有代码的情况下继续”。 

4. 选择“视图” > “Cloud Explorer”。  

5. 在 Cloud Explorer 中选择个人资料图标并登录到 Azure 帐户（如果尚未登录）。

6. 登录后，系统会列出你的 Azure 订阅。 展开包含你的 IoT 中心的订阅。

7. 在该订阅下，依次展开“IoT 中心”和你的 IoT 中心  。 应会看到 IoT 设备的列表，并可以使用 Cloud Explorer 对其进行管理。

   ![在 Cloud Explorer 中访问 IoT 中心资源](./media/tutorial-develop-for-windows/cloud-explorer-view-hub.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>创建新的模块项目

Azure IoT Edge Tools 扩展为 Visual Studio 中支持的所有 IoT Edge 模块语言提供项目模板。 这些模板包含将工作模块部署到测试 IoT Edge 所需的所有文件和代码，或者提供一个起点让你使用自己的业务逻辑自定义模板。

1. 选择“文件” > “新建” > “项目...”   

2. 在“新建项目”窗口中，搜索“IoT Edge”  ，然后选择“Azure IoT Edge (Windows amd64)”项目  。 单击“下一步”  。

   ![创建新的 Azure IoT Edge 项目](./media/tutorial-develop-for-windows/new-project.png)

3. 在“配置新项目”窗口中，重命名项目和解决方案，使名称具有描述性，例如 **CSharpTutorialApp**。 单击“创建”以创建项目。 

   ![配置新的 Azure IoT Edge 项目](./media/tutorial-develop-for-windows/configure-project.png)

4. 在“添加模块”窗口中，使用下列值配置项目：

   | 字段 | Value |
   | ----- | ----- |
   | Visual Studio 模板 | 选择“C# 模块”。  |
   | 模块名称 | 接受默认的 **IotEdgeModule1**。 |
   | 存储库 URL | 映像存储库包含容器注册表的名称和容器映像的名称。 系统已基于模块项目名称值预先填充容器映像。 将 **localhost:5000** 替换为 Azure 容器注册表中的登录服务器值。 可以在 Azure 门户的容器注册表的“概述”页中检索“登录服务器”值。   <br><br> 最终的映像存储库类似于 \<注册表名称\>.azurecr.cn/iotedgemodule1。 |

      ![配置目标设备、模块类型和容器注册表的项目](./media/tutorial-develop-for-windows/add-module-to-solution.png)

5. 选择“添加”  以创建模块。

新项目载入到 Visual Studio 窗口中后，请花费片刻时间来熟悉它所创建的文件：

* 一个名为 **CSharpTutorialApp** 的 IoT Edge 项目。
  * **Modules** 文件夹包含指向项目中模块的指针。 在本例中，应该只有 IotEdgeModule1。
  * 隐藏的 **.env** 文件保存容器注册表的凭据。 这些凭据与 IoT Edge 设备共享，使该设备有权提取容器映像。
  * **deployment.template.json** 文件帮助你创建部署清单的模板。 部署清单文件确切地定义要在设备上部署的模块、模块的配置方式，以及它们如何相互通信以及与云通信。 
    > [!TIP]
    > 在注册表凭据节中，地址是根据你在创建解决方案时提供的信息自动填充的。 但是，用户名和密码引用 .env 文件中存储的变量。 这是出于安全目的，因为 git 会忽略 .env 文件，但不会忽略部署模板。
* 名为 **IotEdgeModule1** 的 IoT Edge 模块项目。
  * **program.cs** 文件中包含项目模板附带的默认 C# 模块代码。 默认模块从源中接收输入，并将其传递给 IoT 中心。
  * **module.json** 文件包含有关模块的详细信息，包括完整的映像存储库、映像版本，以及每个受支持平台使用的 Dockerfile。

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>为 IoT Edge 代理提供注册表凭据

IoT Edge 运行时需要使用注册表凭据将容器映像提取到 IoT Edge 设备中。 IoT Edge 扩展会尝试从 Azure 提取容器注册表信息，并将其填充到部署模板中。

1. 在模块解决方案中打开 **deployment.template.json** 文件。

1. 在 $edgeAgent 所需属性中找到 **registryCredentials** 属性。 它应该会根据你在创建项目时提供的信息自动填写注册表地址，而用户名和密码字段应包含变量名称。 例如：

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "$CONTAINER_REGISTRY_USERNAME_<registry name>",
       "password": "$CONTAINER_REGISTRY_PASSWORD_<registry name>",
       "address": "<registry name>.azurecr.cn"
     }
   }
   ```

1. 在模块解决方案中打开 **.env** 文件。 （默认情况下，此文件会隐藏在解决方案资源管理器中，可能需要选择“显示所有文件”按钮才能显示它。） 

1. 添加从 Azure 容器注册表中复制的“用户名”和“密码”值。  

1. 保存对 .env 文件所做的更改。

### <a name="review-the-sample-code"></a>查看示例代码

创建的解决方案模板包含 IoT Edge 模块的示例代码。 此示例模块只会接收然后传递消息。 管道功能演示了 IoT Edge 中的一个与模块相互通信方式相关的重要概念。

每个模块可以在其代码中声明多个输入和输出队列。   设备上运行的 IoT Edge 中心将消息从一个模块的输出路由到一个或多个模块的输入。 用于声明输入和输出的具体语言各不相同，但在所有模块中的概念是相同的。 有关模块间路由的详细信息，请参阅[声明路由](module-composition.md#declare-routes)。

项目模板附带的示例 C# 代码使用适用于 .NET 的 IoT 中心 SDK 中的 [ModuleClient 类](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)。

1. 在 **program.cs** 文件中，找到 **SetInputMessageHandlerAsync** 方法。

2. [SetInputMessageHandlerAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.setinputmessagehandlerasync?view=azure-dotnet) 方法会设置一个输入队列，用来接收传入消息。 查看此方法，并了解它如何初始化名为 **input1** 的输入队列。

   ![在 SetInputMessageHandlserAsync 构造函数中找到输入名称](./media/tutorial-develop-for-windows/declare-input-queue.png)

3. 接下来，找到 **SendEventAsync** 方法。

4. [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.sendeventasync?view=azure-dotnet) 方法会处理收到的消息，并设置一个输出队列，用来传递这些消息。 查看此方法，可以看到它会初始化名为 **output1** 的输出队列。

   ![在 SendEventAsync 构造函数中找到输出名称](./media/tutorial-develop-for-windows/declare-output-queue.png)

5. 打开 **deployment.template.json** 文件。

6. 查找 $edgeAgent 所需属性的 **modules** 属性。

   此处应会列出两个模块。 第一个模块是 **SimulatedTemperatureSensor**，该模块默认包含在所有模板中，提供可用于测试模块的模拟温度数据。 第二个模块是作为此项目的一部分创建的 **IotEdgeModule1** 模块。

   此模块属性声明要将哪些模块包含到设备部署中。

7. 找到 $edgeHub 所需属性的 **routes** 属性。

   IoT Edge 中心模块的功能之一是在部署中的所有模块之间路由消息。 查看 routes 属性中的值。 第一个路由 **IotEdgeModule1ToIoTHub** 使用通配符 ( **\*** ) 将来自任何输出队列的任何消息包含到 IotEdgeModule1 模块中。 这些消息进入 *$upstream*（用于指示 IoT 中心的保留名称）。 第二个路由 sensorToIotEdgeModule1 接收来自 SimulatedTemperatureSensor 模块的消息，并将其路由到 IotEdgeModule1 模块的 input1 输入队列   。

   ![在 deployment.template.json 中查看路由](./media/tutorial-develop-for-windows/deployment-routes.png)

## <a name="build-and-push-your-solution"></a>生成并推送解决方案

现已查看模块代码和部署模板，并了解了一些重要的部署概念。 现在，已准备好生成 IotEdgeModule1 容器映像并将其推送到容器注册表。 借助适用于 Visual Studio 的 IoT Tools 扩展，此步骤还会根据模板文件中的信息和解决方案文件中的模块信息生成部署清单。

### <a name="sign-in-to-docker"></a>登录到 Docker

向开发计算机上的 Docker 提供容器注册表凭据，以便它可以推送要存储在注册表中的容器映像。

1. 打开 PowerShell 或命令提示符。

2. 使用创建注册表后保存的 Azure 容器注册表凭据登录到 Docker。

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   可能会出现一条安全警告，其中建议使用 `--password-stdin`。 这条最佳做法是针对生产场景建议的，这超出了本教程的范畴。 有关详细信息，请参阅 [docker login](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) 参考。

### <a name="build-and-push"></a>生成并推送

现在，开发计算机以及 IoT Edge 设备都可以访问你的容器注册表。 接下来，可将项目代码转换为容器映像。

1. 右键单击 **CSharpTutorialApp** 项目文件夹，然后选择“生成并推送 IoT Edge 模块”  。

   ![生成并推送 IoT Edge 模块](./media/tutorial-develop-for-windows/build-and-push-modules.png)

   “生成并推送”命令会启动三项操作。 首先，它在解决方案中创建名为 **config** 的新文件夹，用于保存基于部署模板和其他解决方案文件中的信息生成的完整部署清单。 其次，它会运行 `docker build`，以基于目标体系结构的相应 dockerfile 生成容器映像。 然后，它会运行 `docker push`，以将映像存储库推送到容器注册表。

   此过程在首次运行时可能需要花费几分钟时间，但下一次运行这些命令时可以更快地完成。

2. 打开新建的 config 文件夹中的 **deployment.windows-amd64.json** 文件。 （config 文件夹可能未显示在 Visual Studio 中的解决方案资源管理器中。 如果是这样，请在解决方案资源管理器任务栏中选择“显示所有文件”图标。） 

3. 找到 IotEdgeModule1 节的 **image** 参数。 请注意，该映像包含完整的映像存储库，其中附带了来自 module.json 文件的名称、版本和体系结构标记。

4. 打开 IotEdgeModule1 文件夹中的 **module.json** 文件。

5. 更改模块映像的版本号。 （是 version 而不是 $schema-version。）例如，将修补程序版本号递增为 **0.0.2**，就如同我们在模块代码中做了细微的修复一样。

   >[!TIP]
   >模块版本启用版本控制，并可让你在将更新部署到生产环境之前，在少量的设备上测试更改。 如果在生成和推送之前不递增模块版本，则会覆盖容器注册表中的存储库。

6. 保存对 module.json 文件所做的更改。

7. 再次右键单击 **CSharpTutorialApp** 项目文件夹，然后再次选择“生成并推送 IoT Edge 模块”  。

8. 再次打开 **deployment.windows-amd64.json** 文件。 请注意，再次运行“生成并推送”命令时未创建新文件， 而是更新了同一文件以反映更改。 IotEdgeModule1 映像现在指向容器的版本 0.0.2。 在部署清单中做出此项更改的目的是告知 IoT Edge 设备有新的模块版本可供提取。

9. 若要进一步验证“生成并推送”命令执行了哪些操作，请转到 [Azure 门户](https://portal.azure.cn)并导航到你的容器注册表。 

10. 在该容器注册表中，依次选择“存储库”、“iotedgemodule1”。   验证映像的两个版本是否已推送到注册表。

    ![查看容器注册表中的两个映像版本](./media/tutorial-develop-for-windows/view-repository-versions.png)

### <a name="troubleshoot"></a>故障排除

如果在生成和推送模块映像时遇到错误，这些错误往往与开发计算机上的 Docker 配置相关。 使用以下提问来检查配置：

* 是否使用从容器注册表复制的凭据运行了 `docker login` 命令？ 这些凭据不同于用来登录到 Azure 的凭据。
* 你的容器存储库是否正确？ 存储库中是否包含正确的容器注册表名称和模块名称？ 打开 IotEdgeModule1 文件夹中的 **module.json** 文件进行检查。 存储库值应类似于 **\<注册表名称\>.azurecr.cn/iotedgemodule1**。
* 如果为模块使用的名称不是 **IotEdgeModule1**，使用的名称是否在整个解决方案中一致？
* 计算机运行的容器是否与正在生成的容器的类型相同？ 本教程适用于 Windows IoT Edge 设备，因此，Visual Studio 文件应包含 **windows-amd64** 扩展，Docker Desktop 应该运行 Windows 容器。

## <a name="deploy-modules-to-device"></a>将模块部署到设备

确认生成的容器映像已存储在容器注册表中之后，现在可以将其部署到设备。 请确保 IoT Edge 设备已启动并正在运行。

1. 在 Visual Studio 中打开 Cloud Explorer，并展开 IoT 中心的详细信息。

2. 选择要部署到的设备的名称。 在“操作”列表中，选择“创建部署”。  

   ![为单个设备创建部署](./media/tutorial-develop-for-windows/create-deployment.png)

3. 在文件资源管理器，导航到项目的 config 文件夹，然后选择“deployment.windows-amd64.json”文件。  此文件通常位于 `C:\Users\<username>\source\repos\CSharpTutorialApp\CSharpTutorialApp\config\deployment.windows-amd64.json`

   不要使用 deployment.template.json 文件，因为其中不包含完整模块映像的值。

4. 在 Cloud Explorer 中展开 IoT Edge 设备的详细信息，以查看设备上的模块。

5. 使用“刷新”按钮更新设备状态，以查看 SimulatedTemperatureSensor 和 IotEdgeModule1 模块是否已部署到设备  。

   ![查看 IoT Edge 设备上运行的模块](./media/tutorial-develop-for-windows/view-running-modules.png)

## <a name="view-messages-from-device"></a>查看来自设备的消息

IotEdgeModule1 代码通过其输入队列接收消息，然后通过其输出队列传递消息。 部署清单声明了将消息从 SimulatedTemperatureSensor 传递到 IotEdgeModule1，再将消息从 IotEdgeModule1 转发到 IoT 中心的路由。 使用适用于 Visual Studio 的 Azure IoT Edge Tools 可以查看单个设备发出的已抵达 IoT 中心的消息。

1. 在 Visual Studio Cloud Explorer 中，选择已部署到的 IoT Edge 设备的名称。

2. 在“操作”菜单中，选择“开始监视内置事件终结点”。  

3. 观察 Visual Studio 中的“输出”部分，以查看抵达 IoT 中心的消息。 

   启动这两个模块可能需要几分钟时间。 IoT Edge 运行时需要接收其新部署清单、从容器运行时提取模块映像，然后启动每个新模块。

   ![查看传入的设备到云消息](./media/tutorial-develop-for-windows/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>查看设备上的更改

若要查看设备本身上发生的情况，请使用本部分所述的命令来检查设备上运行的 IoT Edge 运行时和模块。

本部分所述的命令适用于 IoT Edge 设备，而不适用于开发计算机。 如果对 IoT Edge 设备使用了虚拟机，现在请连接到该虚拟机。 在 Azure 中，转到该虚拟机的概述页，并选择“连接”以访问远程桌面连接。  在设备上打开命令提示符或 PowerShell 窗口，以运行 `iotedge` 命令。

* 查看已部署到设备的所有模块，并检查其状态：

   ```cmd
   iotedge list
   ```

   应该看到四个模块：两个 IoT Edge 运行时模块、SimulatedTemperatureSensor 和 IotEdgeModule1。 所有四个模块应列为“正在运行”。

* 检查特定模块的日志：

   ```cmd
   iotedge logs <module name>
   ```

   IoT Edge 模块区分大小写。

   SimulatedTemperatureSensor 和 IotEdgeModule1 日志应显示其正在处理的消息。 edgeAgent 模块负责启动其他模块，因此，其日志将包含有关实现部署清单的信息。 如有任一模块未列出或未运行，edgeAgent 日志可能会包含错误。 edgeHub 模块负责模块与 IoT 中心之间的通信。 如果模块已启动并正在运行，但消息未抵达 IoT 中心，edgeHub 日志可能会包含错误。

## <a name="next-steps"></a>后续步骤

在本教程中，在开发计算机上设置了 Visual Studio 2019 并从中部署了第一个 IoT Edge 模块。 了解基本概念后，接下来请尝试将功能添加到模块，使它可以分析其中传递的数据。 选择首选的语言：

> [!div class="nextstepaction"]
> [C](tutorial-c-module-windows.md)
> [C#](tutorial-csharp-module-windows.md)