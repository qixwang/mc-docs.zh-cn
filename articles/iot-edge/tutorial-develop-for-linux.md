---
title: 教程 - 使用 Azure IoT Edge 开发用于 Linux 设备的模块
description: 本教程逐步介绍如何设置开发计算机和云资源，以使用适用于 Linux 设备的 Linux 容器开发 IoT Edge 模块
author: kgremban
manager: philmea
ms.author: v-tawe
origin.date: 11/11/2019
ms.date: 03/30/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 50cfb30381c81f9d72c0ec4363e22967e2c60b3a
ms.sourcegitcommit: 260800ede66f48c886d1426a0fac18b4d402b4f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586660"
---
# <a name="tutorial-develop-iot-edge-modules-for-linux-devices"></a>教程：开发适用于 Linux 设备的 IoT Edge 模块

使用 Visual Studio Code 可以开发代码并将其部署到运行 IoT Edge 的 Linux 设备。

在快速入门中，你已使用 Linux 虚拟机创建了 IoT Edge 设备，并部署了来自 Azure 市场的模块。 本教程逐步介绍如何开发自己的代码并将其部署到 IoT Edge 设备。 本文是学习其他教程的有用先决条件，其他教程将更详细地介绍特定编程语言或 Azure 服务。

本教程使用**将 C# 模块部署到 Linux 设备**的示例。 之所以选择此示例，是因为它是 IoT Edge 解决方案中最常见的开发人员方案。 即使你计划使用其他语言或部署 Azure 服务，本教程仍然有助于了解开发工具和概念。 阅读开发过程的介绍，然后选择偏好的语言或 Azure 服务来深入了解细节。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
>
> * 设置开发计算机。
> * 使用 Visual Studio Code 的 IoT Edge Tools 创建新项目。
> * 将项目生成为容器，并将其存储在 Azure 容器注册表中。
> * 将代码部署到 IoT Edge 设备。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="key-concepts"></a>关键概念

本教程将逐步讲解 IoT Edge 模块的开发。 IoT Edge 模块有时简称模块，它是一个包含可执行代码的容器   。 可将一个或多个模块部署到 IoT Edge 设备。 模块执行特定的任务，例如，从传感器引入数据、清理和分析数据，或者将消息发送到 IoT 中心。 有关详细信息，请参阅[了解 Azure IoT Edge 模块](iot-edge-modules.md)。

开发 IoT Edge 模块时，必须了解开发计算机与该模块最终要部署到的目标 IoT Edge 设备之间的差异。 生成的用于保存模块代码的容器必须与目标设备的操作系统 (OS) 相匹配。  例如，最常见的情形是在 Windows 计算机上开发面向运行 IoT Edge 的 Linux 设备的模块。 在这种情况下，容器操作系统将是 Linux。 在学习本教程的过程中，请注意开发计算机 OS 与容器 OS 之间的差异。  

本教程面向运行 IoT Edge 的 Linux 设备。 只要开发计算机可以运行 Linux 容器，就可以使用首选操作系统。 我们建议使用 Visual Studio Code 对 Linux 设备进行开发，而本教程也正是使用此工具。 还可以使用 Visual Studio，但这两个工具提供的支持存在差异。

下表列出了 Visual Studio Code 和 Visual Studio 支持的 **Linux 容器**开发方案。

|   | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Linux 设备体系结构** | Linux AMD64 <br> Linux ARM32 | Linux AMD64 <br> Linux ARM32 |
| **Azure 服务** | Azure Functions <br> Azure 流分析 <br> Azure 机器学习 |   |
| **语言** | C <br> C# <br> Java <br> Node.js <br> Python | C <br> C# |
| **详细信息** | [适用于 Visual Studio Code 的 Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [适用于 Visual Studio 2017 的 Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) <br> [适用于 Visual Studio 2019 的 Azure IoT Edge 工具](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) |

>[!NOTE]
>公共预览版中提供了对 Linux ARM64 设备的支持。 有关详细信息，请参阅[在 Visual Studio Code（预览版）中开发和调试 ARM64 IoT Edge 模块](https://devblogs.microsoft.com/iotdev/develop-and-debug-arm64-iot-edge-modules-in-visual-studio-code-preview)。

本教程将会讲解 Visual Studio Code 的开发步骤。 如果想要使用 Visual Studio，请参阅[使用 Visual Studio 2019 为 Azure IoT Edge 开发和调试模块](how-to-visual-studio-develop-module.md)中的说明。

## <a name="prerequisites"></a>先决条件

一台开发计算机：

* 可以根据开发偏好，使用自己的计算机或虚拟机。
  * 请确保开发计算机支持嵌套虚拟化。 此功能对于运行容器引擎是必需的，你将在下一部分中安装。
* 大多数可以运行容器引擎的操作系统都可用于开发 Linux 设备的 IoT Edge 模块。 本教程使用 Windows 计算机，但会指出 MacOS 或 Linux 上的已知差异。
* 安装 [Git](https://git-scm.com/)，用于稍后在本教程中提取模块模板包。  
* [适用于 Visual Studio Code 的 C# 扩展（由 OmniSharp 提供支持）](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)。
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)。

Linux 上的一个 Azure IoT Edge 设备：

* 我们建议不要在开发计算机上运行 IoT Edge，而是使用独立的设备。 分开使用开发计算机和 IoT Edge 设备可以更准确地反映真实的部署方案，并且有助于直接体现不同的概念。
* 如果没有另一个可用的设备，请参考快速入门文章使用 [Linux 虚拟机](quickstart-linux.md)在 Azure 中创建一个 IoT Edge 设备。

云资源：

* Azure 中的免费或标准层 [IoT 中心](../iot-hub/iot-hub-create-through-portal.md)。

## <a name="install-container-engine"></a>安装容器引擎

IoT Edge 模块被打包为容器，因此，需要在开发计算机上安装容器引擎来生成和管理容器。 Docker Desktop 提供广泛的功能支持且非常流行，因此我们建议使用它进行开发。 使用 Windows 上的 Docker Desktop 可在 Linux 容器和 Windows 容器之间切换，以便轻松地为不同类型的 IoT Edge 设备开发模块。

参考 Docker 文档在开发计算机上安装：

* [安装适用于 Windows 的 Docker Desktop](https://docs.docker.com/docker-for-windows/install/)

  * 安装适用于 Windows 的 Docker Desktop 时，系统会询问你是要使用 Linux 还是 Windows 容器。 随时可以使用一个方便的开关改变此决定。 本教程使用 Linux 容器，因为我们的模块面向 Linux 设备。 有关详细信息，请参阅[在 Windows 与 Linux 容器之间切换](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)。

* [安装适用于 Mac 的 Docker Desktop](https://docs.docker.com/docker-for-mac/install/)

* 有关多个 Linux 平台上的安装信息，请参阅[关于 Docker CE](https://docs.docker.com/install/)。
  * 对于适用于 Linux 的 Windows 子系统 (WSL)，请安装 Docker Desktop For Windows。

## <a name="set-up-vs-code-and-tools"></a>设置 VS Code 和工具

使用适用于 Visual Studio Code 的 IoT 扩展来开发 IoT Edge 模块。 这些扩展提供项目模板、自动创建部署清单，并可让你监视和管理 IoT Edge 设备。 在本部分，你将安装 Visual Studio Code 和 IoT 扩展，然后设置 Azure 帐户以从 Visual Studio Code 内部管理 IoT 中心资源。

1. 在开发计算机上安装 [Visual Studio Code](https://code.visualstudio.com/)。

2. 安装完成后，选择“视图” > “扩展”。  

3. 搜索 **Azure IoT Tools**，它实际上是一系列的扩展，可帮助你与 IoT 中心和 IoT 设备交互，以及开发 IoT Edge 模块。

4. 选择“安装”  。 包含的每个扩展会单独安装。

5. 扩展安装完成后，选择“视图” > “命令面板”打开命令面板。  

6. 在命令面板中，搜索并选择 **Azure:Sign in** 命令。 根据提示登录到 Azure 帐户。

7. 返回命令面板，搜索并选择 **Azure IoT Hub:Select IoT Hub** 命令。 遵照提示选择 Azure 订阅和 IoT 中心。

8. 打开 Visual Studio Code 的“资源管理器”部分：在左侧的活动栏中选择相应的图标，或选择“视图” > “资源管理器”。  

9. 在“资源管理器”部分的底部，展开已折叠的“Azure IoT 中心设备”菜单。  应会看到上述设备，以及与通过命令面板选择的 IoT 中心相关联的 IoT Edge 设备。

   ![在 IoT 中心查看设备](./media/tutorial-develop-for-linux/view-iot-hub-devices.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>创建新的模块项目

Azure IoT Tools 扩展为 Visual Studio Code 中支持的所有 IoT Edge 模块语言提供项目模板。 这些模板包含将工作模块部署到测试 IoT Edge 所需的所有文件和代码，或者提供一个起点让你使用自己的业务逻辑自定义模板。

本教程使用 C# 模块模板，因为它是最常用的模板。

### <a name="create-a-project-template"></a>创建项目模板

在 Visual Studio Code 命令面板中，搜索并选择 **Azure IoT Edge:** New IoT Edge Solution 命令。 遵照提示操作，并使用以下值创建解决方案：

   | 字段 | Value |
   | ----- | ----- |
   | 选择文件夹 | 在适用于 VS Code 的开发计算机上选择用于创建解决方案文件的位置。 |
   | 提供解决方案名称 | 输入解决方案的描述性名称，或者接受默认的 **EdgeSolution**。 |
   | 选择模块模板 | 选择“C# 模块”。  |
   | 提供模块名称 | 接受默认值“SampleModule”。  |
   | 为模块提供 Docker 映像存储库 | 映像存储库包含容器注册表的名称和容器映像的名称。 容器映像是基于你在上一步中提供的名称预先填充的。 将 **localhost:5000** 替换为 Azure 容器注册表中的登录服务器值。 可以在 Azure 门户的容器注册表的“概览”页中检索登录服务器。 <br><br> 最终的映像存储库类似于 \<注册表名称\>.azurecr.cn/samplemodule。 |

   ![提供 Docker 映像存储库](./media/tutorial-develop-for-linux/image-repository.png)

新解决方案载入到 Visual Studio Code 窗口中后，请花费片刻时间来熟悉它所创建的文件：

* **.vscode** 文件夹包含用于调试模块的名为 **launch.json** 的文件。
* **modules** 文件夹针对解决方案中的每个模块包含一个文件夹。 目前，应该只存在 **SampleModule**（或为模块指定的任何名称）文件夹。 SampleModule 文件夹包含主要程序代码、模块元数据和多个 Docker 文件。
* **.env** 文件保存容器注册表的凭据。 这些凭据与 IoT Edge 设备共享，使该设备有权提取容器映像。
* **deployment.debug.template.json** 文件和 **deployment.template.json** 文件是帮助你创建部署清单的模板。 部署清单文件确切地定义要在设备上部署的模块、模块的配置方式，以及它们如何相互通信以及与云通信。  模板文件使用某些值的指针。 将模板转换为真实的部署清单时，指针将替换为取自其他解决方案文件的值。 在部署模板中找到两个常见的占位符：

  * 在注册表凭据节中，地址是根据你在创建解决方案时提供的信息自动填充的。 但是，用户名和密码引用 .env 文件中存储的变量。 此配置是出于安全考量，因为 .env 文件被 Git 忽略，但部署模板未被忽略。
  * 在 SampleModule 节中，即使创建解决方案时提供了映像存储库，也不会填充容器映像。 此占位符指向 SampleModule 文件夹中的 **module.json** 文件。 如果转到该文件，将会看到映像字段中确实包含了存储库，但同时还有一个由容器版本和平台构成的标记值。 可以在开发周期中手动迭代版本，并使用本部分稍后将会介绍的切换器来选择容器平台。

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>为 IoT Edge 代理提供注册表凭据

环境文件存储容器注册表的凭据，并将其与 IoT Edge 运行时共享。 该运行时需要这些凭据才能将容器映像提取到 IoT Edge 设备中。

IoT Edge 扩展尝试从 Azure 提取容器注册表凭据，并将其填充到环境文件中。 请检查是否已包含你的凭据。 如果未包含，现在请添加这些凭据：

1. 在模块解决方案中打开 **.env** 文件。
2. 添加从 Azure 容器注册表复制的 **username** 和 **password** 值。
3. 保存对 .env 文件所做的更改。

### <a name="select-your-target-architecture"></a>选择目标体系结构

目前，Visual Studio Code 可以为 Linux AMD64 和 ARM32v7 设备开发 C# 模块。 需要选择面向每个解决方案的体系结构，因为这会影响容器的生成和运行方式。 默认设置为 Linux AMD64。

1. 打开命令面板并搜索 **Azure IoT Edge:Set Default Target Platform for Edge Solution**，或者选择窗口底部边栏中的快捷方式图标。

   ![在边栏中选择体系结构图标](./media/tutorial-develop-for-linux/select-architecture.png)

2. 在命令面板中，从选项列表中选择目标体系结构。 本教程将使用 Ubuntu 虚拟机作为 IoT Edge 设备，因此将保留默认设置 **amd64**。

### <a name="review-the-sample-code"></a>查看示例代码

创建的解决方案模板包含 IoT Edge 模块的示例代码。 此示例模块只会接收然后传递消息。 管道功能演示了 IoT Edge 中的一个与模块相互通信方式相关的重要概念。

每个模块可以在其代码中声明多个输入和输出队列。   设备上运行的 IoT Edge 中心将消息从一个模块的输出路由到一个或多个模块的输入。 用于声明输入和输出的具体语言各不相同，但在所有模块中的概念是相同的。 有关模块间路由的详细信息，请参阅[声明路由](module-composition.md#declare-routes)。

项目模板附带的示例 C# 代码使用适用于 .NET 的 IoT 中心 SDK 中的 [ModuleClient 类](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)。

1. 打开 **Program.cs** 文件，该文件位于 **modules/SampleModule/** 文件夹中。

2. 在 program.cs 中，找到 **SetInputMessageHandlerAsync** 方法。

3. [SetInputMessageHandlerAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.setinputmessagehandlerasync?view=azure-dotnet) 方法会设置一个输入队列，用来接收传入消息。 查看此方法，并了解它如何初始化名为 **input1** 的输入队列。

   ![在 SetInputMessageCallback 构造函数中查找输入名称](./media/tutorial-develop-for-linux/declare-input-queue.png)

4. 接下来，找到 **SendEventAsync** 方法。

5. [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.sendeventasync?view=azure-dotnet) 方法会处理收到的消息，并设置一个输出队列，用来传递这些消息。 查看此方法，可以看到它会初始化名为 **output1** 的输出队列。

   ![在 SendEventToOutputAsync 中查找输出名称](./media/tutorial-develop-for-linux/declare-output-queue.png)

6. 打开 **deployment.template.json** 文件。

7. 查找 $edgeAgent 所需属性的 **modules** 属性。

   此处应会列出两个模块。 第一个模块是 **SimulatedTemperatureSensor**，该模块默认包含在所有模板中，提供可用于测试模块的模拟温度数据。 第二个模块是在创建此解决方案过程中创建的 **SampleModule** 模块。

8. 在文件底部，找到 **$edgeHub** 模块的所需属性。

   IoT Edge 中心模块的功能之一是在部署中的所有模块之间路由消息。 查看 **routes** 属性中的值。 第一个路由 **SampleModuleToIoTHub** 使用通配符 ( **\*** ) 指示 SampleModule 模块中的任何输出队列传出的任何消息。 这些消息进入 *$upstream*（用于指示 IoT 中心的保留名称）。 第二个路由 sensorToSampleModule 接收来自 SimulatedTemperatureSensor 模块的消息，并将它们路由到在 SampleModule 代码中初始化的 input1  输入队列。

   ![在 deployment.template.json 中查看路由](./media/tutorial-develop-for-linux/deployment-routes.png)

## <a name="build-and-push-your-solution"></a>生成并推送解决方案

现已查看模块代码和部署模板，并了解了一些重要的部署概念。 接下来，可以生成 SampleModule 容器映像并将其推送到容器注册表。 使用适用于 Visual Studio Code 的 IoT Tools 扩展时，此步骤还会基于模板文件中的信息以及解决方案文件中的模块信息生成部署清单。

### <a name="sign-in-to-docker"></a>登录到 Docker

为 Docker 提供容器注册表凭据，使其可以推送要存储在注册表中的容器映像。

1. 选择“视图” > “终端”，打开 Visual Studio Code 集成终端。  

2. 使用创建注册表后保存的 Azure 容器注册表凭据登录到 Docker。

   ```cmd/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   可能会出现一条安全警告，其中建议使用 `--password-stdin`。 这条最佳做法是针对生产场景建议的，这超出了本教程的范畴。 有关详细信息，请参阅 [docker login](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) 参考。

### <a name="build-and-push"></a>生成并推送

Visual Studio Code 现在有权访问你的容器注册表。接下来，可将解决方案代码转换为容器映像。

1. 在 Visual Studio Code 资源管理器中右键单击“deployment.template.json”文件，然后选择“生成并推送 IoT Edge 解决方案”。  

   ![生成并推送 IoT Edge 模块](./media/tutorial-develop-for-linux/build-and-push-modules.png)

   “生成并推送”命令会启动三项操作。 首先，它在解决方案中创建名为 **config** 的新文件夹，用于保存基于部署模板和其他解决方案文件中的信息生成的完整部署清单。 其次，它会运行 `docker build`，以基于目标体系结构的相应 dockerfile 生成容器映像。 然后，它会运行 `docker push`，以将映像存储库推送到容器注册表。

   此过程在首次运行时可能需要花费几分钟时间，但下一次运行这些命令时可以更快地完成。

2. 打开新建的 config 文件夹中的 **deployment.amd64.json** 文件。 文件名反映了目标体系结构，因此，如果选择了不同的体系结构，则文件名会不相同。

3. 请注意，两个参数的占位符现已填充了适当的值。 **registryCredentials** 节包含从 .env 文件提取的注册表用户名和密码。 **SampleModule** 包含完整的映像存储库，其中附带了来自 module.json 文件的名称、版本和体系结构标记。

4. 打开 SampleModule 文件夹中的 **module.json** 文件。

5. 更改模块映像的版本号。 （是 version 而不是 $schema-version。）例如，将修补程序版本号递增为 **0.0.2**，就如同我们在模块代码中做了细微的修复一样。

   >[!TIP]
   >模块版本启用版本控制，并可让你在将更新部署到生产环境之前，在少量的设备上测试更改。 如果在生成和推送之前不递增模块版本，则会覆盖容器注册表中的存储库。

6. 保存对 module.json 文件所做的更改。

7. 再次右键单击“deployment.template.json”文件，并选择“生成并推送 IoT Edge 解决方案”。  

8. 再次打开 **deployment.amd64.json** 文件。 请注意，再次运行“生成并推送”命令时未创建新文件， 而是更新了同一文件以反映更改。 SampleModule 映像现在指向容器的版本 0.0.2。

9. 若要进一步验证“生成并推送”命令执行了哪些操作，请转到 [Azure 门户](https://portal.azure.cn)并导航到你的容器注册表。 

10. 在该容器注册表中，依次选择“存储库”、“samplemodule”。   验证映像的两个版本是否已推送到注册表。

    ![查看容器注册表中的两个映像版本](./media/tutorial-develop-for-linux/view-repository-versions.png)

<!--Alternative steps: Use VS Code Docker tools to view ACR images with tags-->

### <a name="troubleshoot"></a>故障排除

如果在生成和推送模块映像时遇到错误，这些错误往往与开发计算机上的 Docker 配置相关。 使用以下提问来检查配置：

* 是否使用从容器注册表复制的凭据运行了 `docker login` 命令？ 这些凭据不同于用来登录到 Azure 的凭据。
* 你的容器存储库是否正确？ 存储库中是否包含正确的容器注册表名称和模块名称？ 打开 SampleModule 文件夹中的 **module.json** 文件进行检查。 存储库值应类似于 **\<注册表名称\>.azurecr.cn/samplemodule**。
* 如果为模块使用的名称不是 **SampleModule**，使用的名称是否在整个解决方案中一致？
* 计算机运行的容器是否与正在生成的容器的类型相同？ 本教程适用于 Linux IoT Edge 设备，因此，Visual Studio Code 应会在边栏中显示 **amd64** 或 **arm32v7**，并且 Docker Desktop 应运行 Linux 容器。  

## <a name="deploy-modules-to-device"></a>将模块部署到设备

确认生成的容器映像已存储在容器注册表中之后，现在可以将其部署到设备。 请确保 IoT Edge 设备已启动并正在运行。

1. 在 Visual Studio Code 资源管理器中，展开“Azure IoT 中心设备”部分。

2. 右键单击要部署到的 IoT Edge 设备，然后选择“为单个设备创建部署”。 

   ![为单个设备创建部署](./media/tutorial-develop-for-linux/create-deployment.png)

3. 在文件资源管理器，导航到 **config** 文件夹，然后选择 **deployment.amd64.json** 文件。

   不要使用 deployment.template.json 文件，因为其中不包含容器注册表凭据或模块映像值。 如果面向 Linux ARM32 设备，则部署清单将命名为 deployment.arm32v7.json。

4. 展开 IoT Edge 设备的详细信息，然后展开设备的“模块”列表。 

5. 使用“刷新”按钮更新设备视图，直至看到设备上运行 SimulatedTemperatureSensor 和 SampleModule 模块。

   启动这两个模块可能需要几分钟时间。 IoT Edge 运行时需要接收其新部署清单、从容器运行时提取模块映像，然后启动每个新模块。

   ![查看 IoT Edge 设备上运行的模块](./media/tutorial-develop-for-linux/view-running-modules.png)

## <a name="view-messages-from-device"></a>查看来自设备的消息

SampleModule 代码通过其输入队列接收消息，然后通过其输出队列传递消息。 部署清单声明了从 SimulatedTemperatureSensor 将消息传递到 SampleModule，再将消息从 SampleModule 转发到 IoT 中心的路由。 使用适用于 Visual Studio Code 的 Azure IoT Tools 可以查看单个设备发出的已抵达 IoT 中心的消息。

1. 在 Visual Studio Code 资源管理器中，右键单击想要监视的 IoT Edge 设备，然后选择“开始监视内置事件终结点”  。

2. 观察 Visual Studio Code 中的输出窗口，以查看抵达 IoT 中心的消息。

   ![查看传入的设备到云消息](./media/tutorial-develop-for-linux/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>查看设备上的更改

若要查看设备本身上发生的情况，请使用本部分所述的命令来检查设备上运行的 IoT Edge 运行时和模块。

本部分所述的命令适用于 IoT Edge 设备，而不适用于开发计算机。 如果对 IoT Edge 设备使用了虚拟机，现在请连接到该虚拟机。 在 Azure 中，转到该虚拟机的概述页，并选择“连接”以访问安全外壳连接。 

* 查看已部署到设备的所有模块，并检查其状态：

   ```bash
   iotedge list
   ```

   应该看到四个模块：两个 IoT Edge 运行时模块、SimulatedTemperatureSensor 和 SampleModule。 所有四个模块应列为“正在运行”。

* 检查特定模块的日志：

   ```bash
   iotedge logs <module name>
   ```

   IoT Edge 模块区分大小写。

   SimulatedTemperatureSensor 和 SampleModule 日志应显示其正在处理的消息。 edgeAgent 模块负责启动其他模块，因此，其日志将包含有关实现部署清单的信息。 如有任一模块未列出或未运行，edgeAgent 日志可能会包含错误。 edgeHub 模块负责模块与 IoT 中心之间的通信。 如果模块已启动并正在运行，但消息未抵达 IoT 中心，edgeHub 日志可能会包含错误。

## <a name="next-steps"></a>后续步骤

在本教程中，你已在开发计算机上安装了 Visual Studio Code 并从中部署了第一个 IoT Edge 模块。 了解基本概念后，接下来请尝试将功能添加到模块，使它可以分析其中传递的数据。 选择首选的语言：

> [!div class="nextstepaction"]
> [C](tutorial-c-module.md)
> [C#](tutorial-csharp-module.md)
> [Java](tutorial-java-module.md)
> [Node.js](tutorial-node-module.md)
> [Python](tutorial-python-module.md)