---
title: 对机器学习管道进行调试和故障排除
titleSuffix: Azure Machine Learning
description: 在适用于 Python 的 Azure 机器学习 SDK 中对机器学习管道进行调试和故障排除。 了解开发管道时的常见陷阱，以及有助于在远程执行之前和期间调试脚本的提示。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye
ms.author: trbye
author: trevorbye
ms.date: 10/03/2019
ms.openlocfilehash: 2bbe92445c01c68d6dbd6cb121bd06b2168f9796
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75599601"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>对机器学习管道进行调试和故障排除
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本文介绍如何在 [Azure 机器学习 SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) 中对[机器学习管道](concept-ml-pipelines.md)进行调试和故障排除。

以下部分概述了生成管道时的常见陷阱，以及用于调试管道中运行的代码的不同策略。 如果在使管道按预期运行时遇到问题，请参考以下提示。 

## <a name="testing-scripts-locally"></a>在本地测试脚本

管道中最常见的失败之一是附加的脚本（数据清理脚本、评分脚本等）不按预期方式运行，或者在远程计算上下文中包含运行时错误，而这些错误在 Azure 机器学习工作室中的工作区内难以调试。 

管道本身无法在本地运行，但在本地计算机上的隔离位置运行脚本可以更快地进行调试，因为无需等待计算和环境生成过程完成。 执行此操作需要完成一些开发工作：

* 如果数据位于云数据存储中，则需要下载数据并使其可供脚本使用。 使用较小的数据样本能够很好地在运行时减少系统开销，并快速获取有关脚本行为的反馈
* 如果你正在尝试模拟某个中间管道步骤，可能需要手动生成特定脚本预期前一步骤提供的对象类型
* 还需要定义自己的环境，并复制远程计算环境中定义的依赖项

在本地环境中运行脚本安装后，执行如下所述的调试任务就会容易得多：

* 附加自定义调试配置
* 暂停执行和检查对象状态
* 捕获运行时之前不会公开的类型或逻辑错误

> [!TIP] 
> 确认脚本按预期运行后，合理的下一步是在单步管道中运行该脚本，然后尝试在包含多个步骤的管道中运行该脚本。

## <a name="debugging-scripts-from-remote-context"></a>从远程上下文调试脚本

在开始生成管道之前，在本地测试脚本是调试主要代码段和复杂逻辑的适当方式，但在某个时间点，你可能需要在执行实际管道运行本身期间调试脚本，尤其是在诊断与管道步骤交互期间发生的行为时。 我们建议在步骤脚本中充分使用 `print()` 语句，以便可以查看远程执行期间的对象状态和预期值，就像在调试 JavaScript 代码时一样。

日志文件 `70_driver_log.txt` 包含： 

* 脚本执行期间输出的所有语句
* 脚本的堆栈跟踪 

若要在门户中查找此日志文件和其他日志文件，请先单击工作区中的管道。

![门户中的“管道”页](./media/how-to-debug-pipelines/pipeline-1.png)

导航到管道父运行。

![管道父运行](./media/how-to-debug-pipelines/pipeline-2.png)

单击特定步骤的运行 ID。

![门户中的“管道”页](./media/how-to-debug-pipelines/pipeline-3.png)

导航到“日志”选项卡。  其他日志包含有关环境映像生成过程和步骤准备脚本的信息。

![门户中的“管道”页](./media/how-to-debug-pipelines/pipeline-4.png)

> [!TIP]
> 可在门户上工作区中的“管道”选项卡中找到已发布的管道的运行。   可在“试验”中找到未发布的管道的运行。  

## <a name="troubleshooting-tips"></a>故障排除提示

下表包含管道开发期间出现的一些常见问题，以及可能的解决方法。

| 问题 | 可能的解决方法 |
|--|--|
| 无法将数据传递给 `PipelineData` 字典 | 确保已在脚本中创建了一个目录，该目录对应于管道预期步骤要将数据输出到的位置。 大多数情况下，输入参数将定义输出目录，然后你需要显式创建该目录。 使用 `os.makedirs(args.output_dir, exist_ok=True)` 创建输出目录。 有关演示此设计模式的评分脚本示例，请参阅[该教程](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script)。 |
| 依赖项 bug | 如果在本地开发并测试了脚本，但在管道中的远程计算上运行时发现了依赖项问题，请确保计算环境依赖项和版本与测试环境相匹配。 |
| 计算目标出现不明确的错误 | 删除再重新创建计算目标可以解决计算目标的某些问题。 |
| 管道未重复使用步骤 | 默认已启用步骤重复使用，但是，请确保未在管道步骤中禁用它。 如果已禁用重复使用，则步骤中的 `allow_reuse` 参数将设置为 `False`。 |
| 管道不必要地重新运行 | 为了确保步骤仅在基础数据或脚本发生更改时才重新运行，请解耦每个步骤的目录。 如果对多个步骤使用同一个源目录，则可能会遇到不必要的重新运行。 在管道步骤对象中使用 `source_directory` 参数以指向该步骤的隔离目录，并确保未对多个步骤使用同一个 `source_directory` 路径。 |

## <a name="next-steps"></a>后续步骤

* 有关 [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) 包和 [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) 包的帮助信息，请参阅 SDK 参考。

* 阅读有关使用管道进行批量评分的[高级教程](tutorial-pipeline-batch-scoring-classification.md)。
