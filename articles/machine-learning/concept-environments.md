---
title: 关于 Azure 机器学习环境
titleSuffix: Azure Machine Learning
description: 在本文中，你将了解机器学习环境的优点，在该环境中可以创建跨各种计算目标的可重复、可审核且可移植的机器学习依赖项定义。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: v-yiso
author: trevorbye
origin.date: 01/06/2020
ms.date: 03/09/2020
ms.openlocfilehash: 855a17caea681ce2869d6c1704c3f548996f6ed4
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155322"
---
# <a name="what-are-azure-machine-learning-environments"></a>什么是 Azure 机器学习环境？
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure 机器学习环境指定与训练和评分脚本有关的 Python 包、环境变量和软件设置。 它们还指定运行时（Python、Spark 或 Docker）。 它们是机器学习工作区中受管理且实施了版本控制的实体，可用于创建跨各种计算目标的可再现、可审核且可移植的机器学习工作流。

可以使用本地计算机上的 `Environment` 目标执行以下操作：
* 开发训练脚本。
* 在 Azure 机器学习计算中重用相同的环境进行大规模的模型训练。
* 使用该相同环境部署你的模型。

下图说明了如何将单个 `Environment` 对象同时用于你的运行配置（用于训练）与你的推理和部署配置（用于 Web 服务部署）。

![图示：机器学习工作流中的环境](./media/concept-environments/ml-environment.png)

## <a name="types-of-environments"></a>环境类型

环境可以总体分为三类：特选类、用户管理类以及系统管理类    。

特选环境由 Azure 机器学习提供，且默认可用于你的工作区。 它们包含 Python 包和设置的集合，有助于你开始使用各种机器学习框架。 

在用户管理的环境中，你需要负责设置环境，并在计算目标上安装训练脚本所需的每个包。 Conda 不检查你的环境，也不会为你安装任何组件。 如果你定义自己的环境，则必须将版本为 `>= 1.0.45` 的 `azureml-defaults` 列出为 pip 依赖项。 此包包含将模型作为 Web 服务托管时所需的功能。

如果想让 [Conda](https://conda.io/docs/) 为你管理 Python 环境和脚本依赖项，请使用系统管理的环境。 默认情况下，该服务将采用这种类型的环境，因为它适用于非手动配置的远程计算目标。

## <a name="create-and-manage-environments"></a>创建和管理环境

可以通过以下方式创建环境：

* 定义新的 `Environment` 对象（通过使用特选环境或通过定义自己的依赖项）。
* 使用工作区中的现有 `Environment` 对象。 此方法可以确保你的依赖项的一致性和可再现性。
* 从现有 Anaconda 环境定义中进行导入。
* 使用 Azure 机器学习 CLI

有关特定的代码示例，请参阅[重用环境进行训练和部署](how-to-use-environments.md#create-an-environment)中的“创建环境”部分。 还可以通过工作区轻松管理环境。 它们包括以下功能：

* 提交试验时，环境会自动注册到你的工作区。 还可以手动注册它们。
* 可以从你的工作区中提取环境来用于训练或部署，或对环境定义进行编辑。
* 利用版本控制，你可以查看你的环境随时间的变化，从而确保可再现性。
* 你可以基于你的环境自动生成 Docker 映像。

有关代码示例，请参阅[重用环境进行训练和部署](how-to-use-environments.md#manage-environments)中的“管理环境”部分。

## <a name="next-steps"></a>后续步骤

* 了解如何在 Azure 机器学习中[创建和使用环境](how-to-use-environments.md)。
* 查看 Python SDK 参考文档来了解[环境类](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py)。
* 查看 R SDK 参考文档来了解[环境](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments)。
