---
title: AutoML 试验中的特征化
titleSuffix: Azure Machine Learning
description: 了解 Azure 机器学习提供哪些特征化设置以及自动化机器学习试验如何支持特征工程。
author: nibaccam
ms.author: nibaccam
ms.reviewer: nibaccam
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 05/28/2020
ms.custom: seodec18
ms.openlocfilehash: aeed56c9c082e55fc3cc1e5ea4d79bd25292c578
ms.sourcegitcommit: 1c01c98a2a42a7555d756569101a85e3245732fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85097753"
---
# <a name="featurization-with-automated-machine-learning"></a>使用自动化机器学习进行特征化

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

在本指南中，了解所提供的特征化设置以及如何针对[自动化机器学习试验](concept-automated-ml.md)来自定义这些设置。

特征工程是使用数据领域知识创建特征的过程，这些特征有助于机器学习算法改善学习效果。 在 Azure 机器学习中，应用了数据缩放和规范化技术以便于完成特征工程。 这些技术和特征工程在自动化机器学习 (AutoML) 试验中统称为特征化。

本文假设你已熟悉如何配置 AutoML 试验。 有关详细信息，请参阅以下文章：

* 对于“代码优先”体验：[使用 Python SDK 配置自动化机器学习试验](how-to-configure-auto-train.md)。
* 对于低/无代码体验：[使用 Azure 机器学习工作室创建、查看和部署自动化机器学习模型](how-to-use-automated-ml-for-ml-models.md)

## <a name="configure-featurization"></a>配置特征化

在每一个自动化机器学习试验中，默认情况下都会将[自动缩放和规范化技术](#featurization)应用于数据。 这些缩放和规范化技术是特征化的类型，用于帮助对不同规模数据的特征敏感的算法。 不过，你还可以启用其他特征化，例如缺失值插补、编码和转换 。

> [!NOTE]
> 自动化机器学习特征化步骤（特征规范化、处理缺失数据，将文本转换为数字等）成为了基础模型的一部分。 使用模型进行预测时，将自动向输入数据应用在训练期间应用的相同特征化步骤。

对于使用 SDK 配置的试验，可以启用/禁用 `featurization` 设置，进而可指定应当用于试验的特征化步骤。 如果使用的是 Azure 机器学习工作室，请参阅如何[使用这些步骤](how-to-use-automated-ml-for-ml-models.md#customize-featurization)启用特征化。

下表列出了 [AutoMLConfig 类](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)中 `featurization` 的已接受设置。 

|特征化配置 | 说明|
------------- | ------------- |
|**`"featurization": 'auto'`**| 指示在处理过程中自动执行[数据护栏和特征化步骤](#featurization)。 默认设置。|
|**`"featurization": 'off'`**| 指示不应当自动执行特征化步骤。|
|**`"featurization":`&nbsp;`'FeaturizationConfig'`**| 指示应当使用自定义特征化步骤。 [了解如何自定义特征化](#customize-featurization)。|

<a name="featurization"></a>

## <a name="automatic-featurization"></a>自动特征化

下表汇总了自动应用于数据的技术。 这些技术适用于通过 SDK 或工作室配置的试验。 若要禁用此行为，请在 `AutoMLConfig` 对象中设置 `"featurization": 'off'`。

> [!NOTE]
> 如果计划将自动化 ML 创建的模型导出到 [ONNX 模型](concept-onnx.md)，则仅支持使用 * 指示的特征工程选项（采用 ONNX 格式）。 [详细了解如何将模型转换为 ONNX](concept-automated-ml.md#use-with-onnx)。 

|特征化步骤| 说明 |
| ------------- | ------------- |
|删除高基数或者无差异的特征* |从训练集和验证集中删除这些特征，包括所有值都缺失的特征、在所有行中具有相同值的特征，或具有高基数的特征（例如，哈希、ID 或 GUID）。|
|插补缺少的值* |对于数字特征，将在列中插补平均值。<br/><br/>对于分类特征，将插补最常用值。|
|生成其他特征* |对于日期时间特征：年、月、日、星期、年日期、季、年周、小时、分钟、秒。<br/><br/>对于文本特征：基于单元语法、双元语法和三元字符语法的字词频率。|
|转换和编码*|唯一值较少的数字特征将转换为分类特征。<br/><br/>对于低基数分类特征，将执行独热 (one-hot) 编码；对于高基数特征，将执行独热哈希编码。|
|单词嵌入|使用预先训练的模型将文本标记的向量转换为句子向量的文本特征化器。 文档中每个单词的嵌入向量聚合在一起，以生成文档特征向量。|
|目标编码|对于分类特征，将每个类别映射到回归问题的平均目标值，并映射到分类问题的每个类的类概率。 应用基于频率的加权和 k 折交叉验证，以减少稀疏数据类别导致的映射过度拟合与干扰。|
|文本目标编码|对于文本输入，将使用带有词袋的堆叠线性模型来生成每个类的概率。|
|证据权重 (WoE)|将 WoE 计算为分类列与目标列的关联度量。 它将计算为类内概率与类外概率的比的对数。 此步骤为每个类输出一个数字特征列，无需显式推算缺失值和处理离群值。|
|群集距离|基于所有数字列训练 k 平均聚类模型。  输出 k 个新特征，每个群集一个新的数值特征，其中包含每个样本与每个群集质心的距离。|

## <a name="data-guardrails"></a>数据护栏

数据护栏有助于识别数据的潜在问题（例如缺失值、[类不均衡](concept-manage-ml-pitfalls.md#identify-models-with-imbalanced-data)），并有助于采取纠正措施来改进结果。 

数据护栏适用于 

* SDK 试验 - 当 `AutoMLConfig` 对象中指定了参数 `"featurization": 'auto'` 或 `validation=auto` 时。
* 工作室试验 - 当启用了自动特征化时。  

可通过以下方式查看与试验相关的数据护栏：

* 在使用 Python SDK 提交试验时设置 `show_output=True`。

* 访问工作室中自动化机器学习运行的“数据护栏”选项卡。

### <a name="data-guardrail-states"></a>数据护栏状态

数据护栏将显示以下三种状态之一：“通过”、“完成”或“收到警报”  。

|状态| 说明 |
|----|---- |
|**已通过**| 未检测到任何数据问题，无需用户执行任何操作。 |
|**已完成**| 已对数据应用更改。 我们鼓励用户查看自动化 ML 所采取的纠正措施，以确保所做的更改与预期的结果一致。 |
|**出现警告**| 检测到无法纠正的数据问题。 我们鼓励用户修正和解决该问题。| 

下表描述了当前支持的数据护栏，以及用户在提交试验时可能会显示的相关状态。

护栏|状态|触发器的条件&nbsp;&nbsp;
---|---|---
插补缺少的特征值 |*已通过* <br><br><br> 完成| 在训练数据中未检测到缺失特征值。 详细了解[如何插补缺失值。](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) <br><br> 在训练数据中检测到缺失特征值并进行插补。
高基数特征处理 |*已通过* <br><br><br> 完成| 已分析输入，但未检测到任何高基数特征。 详细了解[高基数特征检测。](#automatic-featurization) <br><br> 在输入中检测到了高基数特征，并进行了处理。
验证拆分处理 |*已完成*| 已将验证配置设置为“自动”，并且训练数据包含的行少于 20,000 行。 <br> 已通过交叉验证来验证经过训练的模型的每个迭代。 详细了解[验证数据。](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#train-and-validation-data) <br><br> 已将验证配置设置为“自动”，并且训练数据包含的行多于 20,000 行。 <br> 输入数据已被拆分成训练数据集和验证数据集，以用于验证模型。
类均衡检测 |*已通过* <br><br><br><br><br> 收到警报 | 输入已经过分析，训练数据中的所有类都是均衡的。 如果某个数据集中每个类都有良好的表示形式（按样本的数量和比率进行度量），则将该数据集视为均衡。 <br><br><br> 在输入中检测到了不均衡类。 若要修复模型偏差，请解决均衡问题。 详细了解[不均衡数据。](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data)
内存问题检测 |*已通过* <br><br><br><br> 完成 |<br> 已分析选定的{范围值、延隔值和滚动窗口值}，但未检测到潜在的内存不足问题。 详细了解时序[预测配置。](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment) <br><br><br>已分析选定的{范围值、延隔值和滚动窗口值}，可能会导致试验内存耗尽。 延隔或滚动窗口配置已关闭。
频率检测 |*已通过* <br><br><br><br> 完成 |<br> 已分析时序，所有数据点都与检测到的频率保持一致。 <br> <br> 已分析时序，检测到了与已检测到的频率不一致的数据点。 这些数据点已从数据集中删除。 详细了解[时序预测的数据准备。](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data)

## <a name="customize-featurization"></a>自定义特征化

你可以自定义特征化设置，以确保用于训练机器学习模型的数据和特征能够产生相关预测。 

若要自定义特征化，请在 `AutoMLConfig` 对象中指定  `"featurization": FeaturizationConfig`。 如果使用 Azure 机器学习工作室进行试验，请参阅[操作方法](how-to-use-automated-ml-for-ml-models.md#customize-featurization)。

支持的自定义项包括：

|自定义|定义|
|--|--|
|列用途更新|替代指定列的特征类型。|
|转换器参数更新 |更新指定转换器的参数。 当前支持 Imputer（平均值、最频繁和中值）和 HashOneHotEncoder。|
|删除列 |要删除进行特征化的列。|
|阻止转换器| 阻止用于特征化进程的转换器。|

使用 API 调用创建 FeaturizationConfig 对象：
```python
featurization_config = FeaturizationConfig()
featurization_config.blocked_transformers = ['LabelEncoder']
featurization_config.drop_columns = ['aspiration', 'stroke']
featurization_config.add_column_purpose('engine-size', 'Numeric')
featurization_config.add_column_purpose('body-style', 'CategoricalHash')
#default strategy mean, add transformer param for for 3 columns
featurization_config.add_transformer_params('Imputer', ['engine-size'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['city-mpg'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['bore'], {"strategy": "most_frequent"})
featurization_config.add_transformer_params('HashOneHotEncoder', [], {"number_of_bits": 3})
```
## <a name="next-steps"></a>后续步骤

* 要了解如何设置自动化机器学习试验，请参阅：

    * 对于代码体验客户：[使用 Azure 机器学习 SDK 配置自动化机器学习试验](how-to-configure-auto-train.md)。
    * 对于低/无代码体验客户：[在 Azure 机器学习工作室中创建自动化机器学习试验](how-to-use-automated-ml-for-ml-models.md)。

* 详细了解[如何以及在何处部署模型](how-to-deploy-and-where.md)。

* 详细了解[如何使用自动化机器学习训练回归模型](tutorial-auto-train-models.md)或[如何在远程资源上使用自动化机器学习进行训练](how-to-auto-train-remote.md)。
