---
title: Visual Studio Code 中的 Azure 机器学习
titleSuffix: Azure Machine Learning
description: 了解如何安装适用于 Visual Studio Code 的 Azure 机器学习，并在 Azure 机器学习中创建试验。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jimgries
author: greazer
ms.date: 09/20/2019
ms.custom: seodec18
ms.openlocfilehash: 1040da997688e18292b191a5ed9edacd2dc4fd34
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75599156"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Azure Machine Learning for Visual Studio Code 入门

本文介绍如何使用“适用于 Visual Studio Code 的 Azure 机器学习”扩展来训练和部署机器学习模型。 

[Azure 机器学习](overview-what-is-azure-ml.md)简化了机器学习模型的生成、训练和部署。
+ 对于训练，它提供本地或远程运行试验的支持。 对于每个试验，可以记录多个运行的自定义指标以微调超参数
+ 还可以使用 Azure 机器学习轻松部署机器学习模型，以满足测试和生产需求。

## <a name="prerequisites"></a>先决条件

+ 如果没有 Azure 订阅，请在开始之前创建一个免费帐户。 试用[免费版或付费版 Azure 机器学习](https://aka.ms/AMLFree)。

+ 安装 [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview) - 可在 Windows、Mac 和 Linux 上运行的轻型代码编辑器。

+ [安装 Python 3.5 或更高版本](https://www.anaconda.com/download/)。


## <a name="install-the-extension"></a>安装扩展

安装 Azure机器学习扩展时，会自动安装另外两个扩展。 它们是 [Azure 帐户扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)和 [Microsoft Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-Python.Python)。 有关使用 Python 扩展编辑、运行和调试 Python 代码的详细信息，请参阅 [Python hello-world 教程](https://code.visualstudio.com/docs/Python/Python-tutorial)。

若要安装 Azure 机器学习扩展：

1. 打开 Visual Studio Code。

1. 切换到“扩展”选项卡，搜索“Azure 机器学习”。

1. 在扩展选项卡上选择“安装”。 

1. 此时会在 Visual Studio Code 中打开该扩展的欢迎选项卡，并且会将 Azure 符号（在以下屏幕截图中以红色突出显示）添加到活动栏。

   ![Visual Studio Code 活动栏中的 Azure 图标](./media/how-to-vscode-tools/azure-activity-bar.png)

1. 在对话框中选择“登录”，然后按提示完成 Azure 身份验证。 

   与“适用于 Visual Studio Code 的 Azure 机器学习”扩展一起安装的“Azure 帐户”扩展可帮助对 Azure 帐户进行身份验证。 有关命令列表，请参阅 [Azure 帐户扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)页。

> [!TIP]
> 也可以直接从[适用于 Visual Studio Code 的 Azure 机器学习扩展（预览版）](https://aka.ms/vscodetoolsforai)下载扩展安装程序。

## <a name="quickstart-with-azure-machine-learning"></a>Azure 机器学习快速入门
可通过多种方式使用 Azure 机器学习来运行训练脚本。 如果你是刚刚开始使用此扩展，请先逐步了解如何快速提交训练脚本，以便在 Azure 中运行。

如果你对 Azure 机器学习的概念有一定的了解，并希望更详细地了解如何在扩展中管理和使用这些概念，请参阅下面的 [VS Code 中的 Azure 机器学习的深度知识](how-to-vscode-tools.md#azure-machine-learning-in-depth-with-vs-code)。

## <a name="run-an-existing-python-training-script-in-azure"></a>在 Azure 中运行现有的 Python 训练脚本
如果你有现有的训练脚本，适用于 VS Code 的 Azure 机器学习扩展不仅可以提供出色的编辑、调试和源代码管理体验，而且还能让你轻松地在 Azure 中运行和存储脚本的指标。

让我们开始吧。 准备好自己的训练脚本后，可以开始使用该脚本，或克隆 [vscode-tools-for-ai 存储库](https://github.com/microsoft/vscode-tools-for-ai)示例。 这是一个公共存储库，其中归档了有关此扩展的问题。 其中还包含一个较小的 **mnist** 示例文件夹，稍后我们将在本示例中使用。

1. 在 VS Code 中打开 **mnist** 文件夹。

1. 使用偏好的虚拟环境包或 Anaconda 创建新的 Python 环境，并安装 TensorFlow 和 numpy 包。

1. 在 VS Code 状态栏的左下角，选择作为 Python 解释器创建的新环境。

1. 打开 **train.py**，然后打开调试器并按运行按钮（或按 F5）运行该文件。

   [![运行 MNIST 训练](./media/how-to-vscode-tools/run-mnist.gif)](./media/how-to-vscode-tools/run-mnist.gif#lightbox)

如果正确安装了所有组件，则该脚本将会运行，并在 outputs 文件夹中创建一个 TensorFlow 模型。

[![显示 TensorFlow 模型](./media/how-to-vscode-tools/show-tensorflow-model.gif)](./media/how-to-vscode-tools/show-tensorflow-model.gif#lightbox)

知道脚本可运行正常后，接下来让我们在 Azure 中运行它！

无需对 **train.py** 进行额外的修改，即可轻松运行。 但是，只需完成几项简单的更改，就能利用 Azure 机器学习来自动跟踪所选的有关每个训练运行的重要指标。

### <a name="make-azure-aware-of-your-run-metrics"></a>使 Azure 能够识别你的运行指标
若要修改项目，使 Azure 能够识别运行中的重要信息：

1. 在 **train.py** 所在的同一文件夹中创建名为 **amlrun.py** 的文件

    ```Python
    import azureml
    from azureml.core import Run

    # Access the Azure ML run
    # Init run param to check if running within AML
    def get_AMLRun():
        try:
            run = Run.get_submitted_run()
            return run
        except Exception as e:
            print("Caught = {}".format(e.message))
            return None
    ```

2. 在 **train.py** 中导入 amlrun 文件

    ```Python
    ...
    from utils import prepare_data
    from amlrun import get_AMLRun
    ...
    ```
3. 初始化 **train.py** 中的 run 对象

    ```Python
    ...
    init = tf.global_variables_initializer()
    saver = tf.train.Saver()
    run = get_AMLRun()
    ...
    ```
4. 使用 run.log() 函数将指标记录到 Azure：

    ```Python
    ...
            acc_val = acc_op.eval(feed_dict = {X: X_test, y: y_test})

            # Log accuracies to AML logger if using AML
            if run != None:
                run.log('Validation Accuracy', np.float(acc_val))
                run.log('Training Accuracy', np.float(acc_train))

            print(epoch, '-- Training accuracy:', acc_train, '\b Validation
    ...
    ```
### <a name="run-the-script-in-azure"></a>在 Azure 中运行脚本
就这么简单！ 现在，只需使用扩展即可在云中运行脚本！ 请注意，以下演练视频可让你自由压缩创建新 Azure 机器学习工作区和计算资源所需的时间，以及运行训练脚本所需的时间。

   [![启动 Azure ML 试验](./media/how-to-vscode-tools/start-golden-path.gif)](./media/how-to-vscode-tools/start-golden-path.gif#lightbox)

单击“运行试验”按钮后，按如下所示回答提示问题：

1. 选择自己的 Azure 订阅。
1. 选择创建新的 Azure 机器学习工作区。 
1. 从一组预配置的模板中进行选择，以初始化运行的 Python 环境。 这些模板提供一个起点，并包含以下各项的设置：
    1. “PyTorch”、“TensorFlow”或“Scikit-learn”   
    1. “单一”或“分布式”计算训练  
    1. 自定义环境的“常规”设置 
1. 添加模板中未包含的任何包，确保脚本的 pip 和 conda 包列表是完整的。
1. 查看试验运行的默认名称和规范，然后单击 JSON 文件中的“Submit Experiment”链接。  不会保存该 JSON 文件，它只是用于在提交试验之前检查或更改试验设置。
1. 休息片刻，让扩展自动完成所有设置并运行脚本！

    [![在云中训练](./media/how-to-vscode-tools/run-golden-path.gif)](./media/how-to-vscode-tools/run-golden-path.gif#lightbox)

几秒钟后，系统会通知已将试验提交到 Azure，此时，可以通过在 Azure 机器学习工作室中单击 VS Code 通知中的“查看试验运行”链接，或者在 VS Code 中点击“Azure”选项卡中的刷新按钮，来查看试验的进度。 

目前，仅支持在工作室中查看运行指标。 单击上面提到的“查看试验运行”链接会转到运行，在其中可看到记录的指标。 
[![在门户中运行的试验](./media/how-to-vscode-tools/experiment-run-on-portal.PNG)](./media/how-to-vscode-tools/experiment-run-on-portal.PNG#lightbox)

## <a name="azure-machine-learning-in-depth-with-vs-code"></a>VS Code 中的 Azure 机器学习的深度知识

在上面的演练中，我们已按最方便的途径提交了一个试验。 你可能已注意到，扩展可以最大限度地减少运行试验所需的步骤。 本部分将介绍如何单独管理所有的 Azure 机器学习概念，同时提供最高的控制度。

在 Visual Studio Code 中开始训练和部署机器学习模型之前，需要在云中创建一个 [Azure 机器学习工作区](concept-workspace.md)。 此工作区将包含你的模型和资源。

### <a name="create-a-workspace"></a>创建工作区

1. 在 Visual Studio Code 活动栏上，选择 Azure 图标。 此时会显示“Azure 机器学习”边栏。

    [![创建工作区](./media/how-to-vscode-tools/create-workspace.gif)](./media/how-to-vscode-tools/create-workspace.gif#lightbox)


1. 右键单击你的 Azure 订阅，然后选择“创建工作区”。  默认会生成一个包含创建日期和时间的名称。 请将该名称更改为 **TeamWorkspace**，然后按 Enter。

1. 从列表中选择一个资源组（如果知道要选择哪一个），或创建一个新资源组。 若要创建新资源组，请选择最靠近模型部署位置的位置。 

1. 按 Enter 后，Azure 机器学习会收到创建工作区的请求。 Visual Studio Code 通知区域中会显示创建过程的通知。

1. 展开“订阅”节点即可找到新建的工作区。

### <a name="create-an-experiment"></a>创建试验
可以在工作区中创建一个或多个试验，以跟踪和分析各个模型训练运行。 可以在 Azure 云或本地计算机中完成运行。

1. 展开“TeamWorkspace”工作区。  右键单击“试验”节点，并从上下文菜单中选择“创建试验”。  

1. 在提示符下，输入试验的名称。 在示例屏幕截图中，试验名为 **MNIST**。

1. 按 Enter 以创建新试验。 新试验在树中显示为“试验”节点的子级。 

1. 在工作区中，可以右键单击某个试验以将其设置为“活动”试验。  “活动”试验将云中的试验链接到当前已在 Visual Studio Code 中打开的文件夹。  此文件夹应包含本地 Python 脚本。 设置活动试验后，所有训练运行的关键指标将存储在试验中，而不管这些运行在何处执行。

    [![创建试验](./media/how-to-vscode-tools/create-experiment.gif)](./media/how-to-vscode-tools/create-experiment.gif#lightbox)


### <a name="create-and-manage-compute-targets"></a>创建和管理计算目标

使用适用于 Visual Studio Code 的 Azure 机器学习可以准备数据、训练模型，以及在本地和远程计算目标上部署模型。

该扩展支持 Azure 机器学习的多个远程计算目标。 有关详细信息，请参阅 [Azure 机器学习支持的计算目标](how-to-set-up-training-targets.md)完整列表。

### <a name="create-compute-targets-for-azure-machine-learning-in-visual-studio-code"></a>在 Visual Studio Code 中创建 Azure 机器学习的计算目标

若要创建计算目标：

1. 在 Visual Studio Code 活动栏上，选择 Azure 图标。 此时会显示“Azure 机器学习”边栏。

1. 在树视图中，展开你的 Azure 订阅和 Azure 机器学习工作区。

1. 在工作区节点下，右键单击“计算”  节点，再选择“创建计算”  。

1. 从列表中选择计算目标类型。

1. 在命令面板提示窗口中选择一个虚拟机大小。 可以使用类似于“gpu”的文本筛选计算。

1. 在命令面板提示窗口中输入计算目标的名称。

1. 输入名称后，将使用默认参数创建计算。 若要更改参数，请右键单击新的计算，并选择“编辑计算”。 

1. 在显示的 JSON 中进行所需的更改，然后单击“保存并继续”CodeLens（如果使用键盘，可以按 **Ctrl-Shift-P** 调用命令面板，然后运行“Azure ML:  保存并继续”命令）

以下示例演示如何创建和编辑 Azure 机器学习计算 (AMLCompute)：

[![在 Visual Studio Code 中创建 AML 计算](./media/how-to-vscode-tools/create-remote-compute.gif)](./media/how-to-vscode-tools/create-remote-compute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>运行配置文件

若要在某个计算中运行 Azure 机器学习试验，需要正确配置该计算。 运行配置文件是用于指定此环境的机制。

以下示例演示如何为前面创建的 AmlCompute 创建运行配置。

[![为计算创建运行配置](./media/how-to-vscode-tools/create-runconfig.gif)](./media/how-to-vscode-tools/create-runconfig.gif#lightbox)

若要在本地计算机上运行 Azure ML 试验，仍然需要一个运行配置文件。 创建本地运行配置时，所用的 Python 环境默认采用 VS Code 中设置的解释器的路径。

### <a name="train-and-tune-models"></a>定型和优化模型

使用适用于 VS Code 的 Azure ML 扩展，可以通过多种方式在试验中运行训练脚本。

1. 右键单击训练脚本并选择“Azure ML:  在 Azure 中作为试验运行”
1. 单击“运行试验”工具栏图标。
1. 右键单击某个运行配置节点。
1. 使用 VS Code 命令面板执行“Azure ML:  运行试验”

若要运行 Azure 机器学习试验：

1. 在 Visual Studio Code 活动栏上，选择 Azure 图标。

1. 在树视图中，展开你的 Azure 订阅和 Azure 机器学习工作区。

1. 在工作区节点下展开“试验”节点，然后右键单击要运行的试验。 

1. 选择“运行试验”  。

1. 选择要运行的 Python 文件名以训练模型，然后按 Enter 提交运行。 注意：所选的文件必须位于当前已在 VS Code 中打开的文件夹中。

1. 提交运行后，一个“运行”节点将显示在所选的试验下。  使用此节点可以监视运行的状态。 注意：可能需要定期刷新窗口才能看到最新状态。

以下示例演示如何对前面创建的计算运行试验：

[![在本地运行试验](./media/how-to-vscode-tools/run-experiment.gif)](./media/how-to-vscode-tools/run-experiment.gif#lightbox)

### <a name="deploy-and-manage-models"></a>部署和管理模型
在 Azure 机器学习中，可以在云中与边缘部署和管理机器学习模型。

#### <a name="register-your-model-to-azure-machine-learning-from-visual-studio-code"></a>在 Visual Studio Code 将模型注册到 Azure 机器学习

训练模型后，可将其注册到工作区中。 可以跟踪和部署已注册的模型。

若要注册模型：

1. 在 Visual Studio Code 活动栏上，选择 Azure 图标。 此时会显示“Azure 机器学习”边栏。

1. 在树视图中，展开你的 Azure 订阅和 Azure 机器学习工作区。

1. 在工作区节点下，右键单击“模型”  ，再选择“注册模型”  。

1. 在命令面板上的字段中，输入模型名称。

1. 在列表中，选择是要上传**模型文件**（对于单一模型）还是**模型文件夹**（对于包含多个文件的模型，例如 Tensorflow）。

1. 选择你的文件夹或文件。

1. 完成模型属性的配置后，在窗口的右下角选择“提交”。 

以下示例演示如何将模型注册到 Azure 机器学习：

[![将模型注册到 AML](./media/how-to-vscode-tools/register-model.gif)](./media/how-to-vscode-tools/register-model.gif#lightbox)


#### <a name="deploy-your-service-from-visual-studio-code"></a>从 Visual Studio Code 部署服务

在 Visual Studio Code 中，可将 Web 服务部署到：
+ Azure 容器实例 (ACI) 用于测试。
+ Azure Kubernetes 服务 (AKS) 用于生产。

无需事先创建 ACI 容器即可进行测试，因为可按需创建 ACI 容器。 但是，需要提前配置 AKS 群集。 有关详细信息，请参阅[使用 Azure 机器学习部署模型](how-to-deploy-and-where.md)。

若要部署 Web 服务：

1. 在 Visual Studio Code 活动栏上，选择 Azure 图标。 此时会显示“Azure 机器学习”边栏。

1. 在树视图中，展开你的 Azure 订阅和 Azure 机器学习工作区。

1. 在工作区节点下，展开“模型”  节点。

1. 右键单击要部署的模型，并从上下文菜单中选择“部署已注册模型中的服务”。 

1. 在命令面板上，选择要部署到的计算目标。

1. 在命令面板上的字段中，输入此服务的名称。

1. 在命令面板上，按键盘上的 Enter 键以浏览并选择脚本文件。

1. 在命令面板上，按键盘上的 Enter 键以浏览并选择 conda 依赖项文件。

1. 完成服务属性的配置后，在窗口的右下角选择“提交”进行部署。  在服务属性文件中，可以指定本地 Docker 文件或 schema.json 文件。

此时，Web 服务已部署。

以下示例演示如何部署 Web 服务：

[![部署 Web 服务](./media/how-to-vscode-tools/create-image.gif)](./media/how-to-vscode-tools/create-image.gif#lightbox)

### <a name="experiment-with-additional-features"></a>试验和其他功能

可以使用命令面板来访问 Visual Studio Code 中的许多 Azure 机器学习功能。 若要调用命令面板，请按 Ctrl+Shift+P。 在此处可以搜索扩展的其他 Azure ML 功能。

[![适用于 Visual Studio Code 的 Azure 机器学习的快捷键](./media/how-to-vscode-tools/commands.gif)](./media/how-to-vscode-tools/commands.gif#lightbox)

## <a name="next-steps"></a>后续步骤

* 有关如何在 Visual Studio Code 外部使用 Azure 机器学习进行训练的演练，请参阅[教程：使用 Azure 机器学习训练模型](tutorial-train-models-with-aml.md)。
* 有关如何在本地编辑、运行和调试代码的演练，请参阅 [Python hello-world 教程](https://code.visualstudio.com/docs/Python/Python-tutorial)。
