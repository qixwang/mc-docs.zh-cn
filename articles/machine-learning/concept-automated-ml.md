---
title: 什么是自动化 ML/AutoML
titleSuffix: Azure Machine Learning
description: 了解 Azure 机器学习如何自动选取算法，以及如何通过它来生成模型，以便使用你提供的参数和条件为模型选择最佳算法，从而节省时间。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cartacioS
ms.author: v-yiso
origin.date: 11/04/2019
ms.date: 03/09/2020
ms.openlocfilehash: dde8c8ccb31d73b7b1457026631bd937c0c9a12c
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79292388"
---
# <a name="what-is-automated-machine-learning"></a>什么是自动化机器学习？

自动化机器学习也称为自动化 ML，是将机器学习模型开发过程中耗时的反复性任务自动化的过程。 数据科学家、分析师和开发人员可以使用它来生成高度可缩放、高效且高产能的 ML 模型，同时保证模型的质量。 自动化 ML 基于 [Microsoft Research 部门](https://arxiv.org/abs/1705.05355)的突破性技术。

传统的机器学习模型开发是资源密集型的，需要具备丰富的领域知识，并需要花费大量的时间来生成和比较数十个模型。 想要通过 Azure 机器学习使用指定的目标指标训练和优化模型时，可以运用自动化 ML。 然后，该服务将迭代与特征选择配对的 ML 算法，每次迭代都会生成模型和训练评分。 模型的评分越高，则认为它可以更好地“拟合”数据。

使用自动化机器学习可以缩减生成生产就绪型 ML 模型所需的时间，同时使工作变得更轻松、更高效。

## <a name="when-to-use-automated-ml"></a>何时使用自动化 ML

自动化 ML 可将机器学习模型开发过程标准化，并使其用户（无论是否具备数据科学知识）能够在端到端的机器学习管道中识别任何问题。

跨行业的数据科学家、分析师和开发人员可以使用自动化 ML 来实现以下目的：

+ 无需丰富的编程知识，即可实现机器学习解决方案
+ 节省时间和资源
+ 利用数据科学最佳做法
+ 提供灵活的问题解决方法

下表列出了常见的自动化 ML 用例。 

分类| 时序预测 | 回归
---|---|---
[欺诈检测](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb)|[销售预测](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb)|[CPU 性能预测](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-hardware-performance-explanation-and-featurization/auto-ml-regression-hardware-performance-explanation-and-featurization.ipynb)
|[营销预测](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)|[需求预测](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)|
|[新闻组数据分类](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb)|[饮料生产预测](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)|

## <a name="how-automated-ml-works"></a>自动化 ML 的工作原理

使用 **Azure 机器学习**可以通过以下步骤设计和运行自动化 ML 训练试验：

1. **识别要解决的 ML 问题**：分类、预测或回归

1. **指定已标记训练数据的源和格式**：Numpy 数组或 Pandas 数据帧

1. **配置模型训练的计算目标**，例如[本地计算机、Azure 机器学习计算、远程 VM 或 Azure Databricks](how-to-set-up-training-targets.md)。  了解如何对[远程资源](how-to-auto-train-remote.md)进行自动训练。

1. **配置自动化机器学习参数**，用于确定要对不同模型运行的迭代次数、超参数设置、高级预处理/特征化，以及在确定最佳模型时要查看的指标。  可以在 [Azure 机器学习工作室](https://ml.azure.com)中或使用 [SDK](how-to-configure-auto-train.md) 配置自动训练试验的设置。 

    [!INCLUDE [aml-applies-to-enterprise-sku](../../includes/aml-applies-to-enterprise-sku-inline.md)]

1. **提交训练运行。**

  ![自动化机器学习](./media/concept-automated-ml/automl-concept-diagram2.png)

在训练期间，Azure 机器学习会创建多个尝试不同算法和参数的并行管道。 一旦达到试验中定义的退出条件，机器学习就会停止。

还可以检查记录的运行信息，其中包含运行期间收集的[指标](how-to-understand-automated-ml.md)。 训练运行会生成一个包含模型和数据预处理的 Python 序列化对象（`.pkl` 文件）。

模型生成是自动化的，同时，你也可以[了解特征对于生成的模型而言如何重要或者彼此相关](how-to-configure-auto-train.md#explain)。

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>预处理

在每个自动化机器学习试验中，数据将使用默认方法进行预处理，有时还会通过高级预处理技术进行预处理。

> [!NOTE]
> 自动机器学习预处理步骤（特征规范化、处理缺失数据，将文本转换为数字等）成为基础模型的一部分。 使用模型进行预测时，训练期间应用的相同预处理步骤将自动应用于输入数据。

### <a name="automatic-preprocessing-standard"></a>自动预处理（标准）

在每个自动化机器学习试验中，数据将自动缩放或规范化，以帮助确保算法的良好性能。  在模型训练过程中，将对每个模型应用以下缩放或规范化技术之一。

|缩放和规范化| 说明 |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | 通过删除平均值并缩放到单位差异来标准化特征  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | 通过按该列的最小值和最大值缩放每个特征来转换特征  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |按特征的最大绝对值缩放每个特征 |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |按特征的分位数范围缩放特征 |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |使用数据的单值分解进行线性维度化简，以将其投影到低维空间 |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |此转换器通过截断的单值分解 (SVD) 执行线性维度化简。 与 PCA 相反，此估算器在计算单值分解之前不会将数据居中，这意味着它可以有效地处理 scipy.sparse 矩阵 |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | 重新缩放至少包含一个非零成分的每个样本（即，数据矩阵的每个行），而不管其他样本如何，使其范数（l1 或 l2）等于 1 |

### <a name="advanced-preprocessing-optional-featurization"></a>高级预处理：可选特征化

还可以使用其他高级预处理和特征化，例如数据护栏、编码和转换。 [详细了解包含的功能化](how-to-create-portal-experiments.md#featurization)。 可通过以下方式启用此设置：

+ Azure 机器学习工作室：[通过以下步骤](how-to-create-portal-experiments.md#create-and-run-experiment)在“查看其他配置”  部分中启用“自动特征化”。 

+ Python SDK：为 [`AutoMLConfig` 类](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)指定 `"feauturization": 'auto' / 'off' / 'FeaturizationConfig'`。 

## <a name="prevent-over-fitting"></a>防止过度拟合

如果模型过于完美地拟合训练数据，因而无法准确预测不可见的测试数据，则机器学习中就会发生过度拟合。 换言之，模型只是记住训练数据中的特定模式和干扰，但无法足够灵活地基于实际数据做出预测。 在最严重的情况下，过度拟合的模型会假设训练过程中出现的特征值组合始终为目标生成完全相同的输出。 

防止过度拟合的最佳方式是遵循 ML 最佳做法，包括：

* 使用更多训练数据，并消除统计偏差
* 防止目标泄漏
* 使用较少的特征
* **正则化和超参数优化**
* **模型复杂性限制**
* **交叉验证**

在自动化 ML 的上下文中，上面所述的前三项是**你要实施的最佳做法**。 带粗体格式的后三项是**自动化 ML 为了防止过度拟合而默认实施的最佳做法**。 在除自动化 ML 以外的设置中，为了避免过度拟合模型，值得遵循所有六项最佳做法。

### <a name="best-practices-you-implement"></a>你要实施的最佳做法

使用**更多的数据**是防止过度拟合的最简单且最可行的方法，通常，这种做法带来的额外好处就是提高准确度。 使用更多数据时，模型将更难以记住确切的模式，因此它被迫达成可以更灵活地适应更多条件的解决方案。 此外，必须识别**统计偏差**，以确保训练数据不包含实时预测数据中不存在的隔离模式。 此问题可能很难解决，因为训练集和测试集之间可能不存在过度拟合，但与实时测试数据相比，可能又存在过度拟合。

目标泄漏是一个类似的问题，在这种情况下，可能在训练/测试集之间看不到过度拟合，但在预测时却会出现过度拟合。 如果模型在训练过程中“作弊”（访问预测时一般不会存在的数据），则会发生目标泄漏。 例如，如果你的问题是在星期一预测某件商品在星期五的价格，但某个特征意外地包含星期四的数据，则预测时模型不会包含此数据，因为模型无法看到未来。 目标泄漏是一个很容易疏忽的错误，但问题的准确度异常高，则往往可以体现此错误。 如果你正在尝试预测股价，并以 95% 的准确度训练了模型，则特征中的某个位置很可能出现目标泄漏。

删除特征也有助于避免过度拟合问题：防止在模型中包含过多的字段用于记住特定的模式，从而使其更灵活。 以量化方式进行度量可能有难度，但如果可以删除特征并保持相同的准确度，则有可能会使模型变得更灵活，并降低过度拟合的风险。

### <a name="best-practices-automated-ml-implements"></a>自动化 ML 实施的最佳做法

正则化是最大程度地减小代价函数来惩罚复杂的过度拟合模型的过程。 有多种不同类型的正则化函数，但一般情况下，这些函数全都惩罚模型系数大小、差异和复杂性。 自动化 ML 结合用于控制过度拟合的不同模型超参数设置，使用 L1 (Lasso)、L2 (Ridge) 和 ElasticNet（同时包括 L1 和 L2）的不同组合。 简单而言，自动化 ML 会根据模型的调控程度而改变，并选择最佳结果。

自动化 ML 还实施明确的模型复杂性限制来防止过度拟合。 大多数情况下，此实现专用于决策树或林算法，其中每个树的最大深度会受到限制，林或系综技术中使用的树总数会受到限制。

交叉验证 (CV) 是提取整个训练数据的多个子集，并针对每个子集训练模型的过程。 其思路是，某个模型可能比较“幸运”，在预测一个子集时具有很高的准确度，但使用许多子集时，模型无法每次都实现这种高准确度。 执行 CV 时，需要提供一个验证维持数据集，指定 CV 折数（子集数），然后，自动化 ML 将训练模型并优化超参数，以尽量减少验证集的错误。 可能有一个 CV 折过度拟合，但如果使用许多的折，则可以减少最终模型过度拟合的可能性。 缺点是 CV 会导致训练时间变得更长，从而增大成本，因为模型不是训练一次，而是针对 *n* 个 CV 子集中的每个子集训练一次。

> [!NOTE]
> 默认不会启用交叉验证；必须在自动化 ML 设置中进行配置。 但是，在配置 CV 并提供验证数据集后，此过程会自动化。

### <a name="identifying-over-fitting"></a>识别过度拟合

考虑以下训练的模型及其相应的训练和测试准确度。

| 型号 | 训练准确度 | 测试准确度 |
|-------|----------------|---------------|
| A | 99.9% | 95% |
| B | 87% | 87% |
| C | 99.9% | 45% |

考虑模型 **A**：一个常见的误解是，如果针对不可见数据的测试准确度低于训练准确度，则模型就是过度拟合的。 但是，测试准确度应始终小于训练准确度，并且过度拟合与适当拟合的差别以较不准确的程度为准。  

将模型 **A** 与 **B** 相比较时，模型 **A** 是更好的模型，因为它的测试准确度更高；尽管测试准确度略低于 95%，但这种差异并不明显，也并不意味着存在过度拟合。 不选择模型 **B** 的原因仅仅是训练和测试精度更为接近。

模型 **C** 明确反映了过度拟合的情况；训练准确度极高，但测试准确度远远达不到较高的水平。 这种差别是主观性的，但需要根据对问题和数据的认知，以及可接受的错误程度来判断。 

## <a name="time-series-forecasting"></a>时序预测

生成预测是任何业务（无论是收入、库存、销售还是客户需求）中不可或缺的组成部分。 可以使用自动化 ML 来合并多种技术和方法，获得推荐的高质量时序预测结果。

自动化时序试验被视为多元回归问题。 将“透视”过去的时序值，使其成为回归量与其他预测指标的附加维度。 与传统时序方法不同，这种方法的优点是，在训练过程中自然包含多个上下文变量及其相互关系。 自动化 ML 会针对数据集和预测时间范围内的所有项目，习得通常有内部分支的单个模型。 这样就可以使用更多的数据来估计模型参数，使得未知系列的泛化成为可能。

详细了解[用于时序预测的自动化机器学习](how-to-auto-train-forecast.md)并查看示例。 或者，请参阅[能源需求笔记本](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)中的高级预测配置的详细代码示例，包括：

* 假日检测和特征化
* 时序和 DNN 教学器（Auto-ARIMA、Prophet、ForecastTCN）
* 通过分组实现的多模型支持
* 滚动原点交叉验证
* 可配置滞后
* 滚动窗口聚合特征

## <a name="ensemble"></a> 系综模型

自动化机器学习支持默认已启用的系综模型。 系综学习通过组合多个模型而不是使用单个模型，来改善机器学习结果和预测性能。 系综迭代显示为运行的最后一个迭代。 自动化机器学习使用投票和堆叠系综方法来组合模型：

* **投票**：根据预测类概率（对于分类任务）或预测回归目标（对于回归任务）的加权平均值进行预测。
* **堆叠**：堆叠方法组合异构的模型，并根据各个模型的输出训练元模型。 当前的默认元模型是 LogisticRegression（对于分类任务）和 ElasticNet（对于回归/预测任务）。

提供排序系综初始化的 [Caruana 系综选择算法](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf)用于决定要在系综中使用的模型。 从较高层面看，此算法使用个体评分最高的最多五个模型来初始化集成，并验证这些模型是否在最佳评分的 5% 阈值范围内，以避免初始系综不佳。 然后，对于每个系综迭代，会将一个新模型添加到现有系综，并计算最终评分。 如果新模型改善了现有的系综评分，则会更新系综以包含新模型。

请参阅[操作指南](how-to-configure-auto-train.md#ensemble)来了解如何在自动化机器学习中更改默认系综设置。

## <a name="imbalance"></a> 不平衡的数据

不平衡的数据往往出现在机器学习分类方案的数据中，指的是在每个类中包含不相称比例的观测值的数据。 这种不平衡可能会对模型准确度造成错误的认知效应，因为输入数据与一个类存在偏差，从而导致训练的模型模拟该偏差。 

自动化 ML 的目标之一是简化机器学习工作流，其中的内置功能可帮助处理不平衡的数据，例如， 

- **权重列**：自动化 ML 支持将加权列用作输入，以便能够增大或减小数据中的行的权重，使某个类的“重要性”更大或更小。

- 自动化 ML 使用的算法可以正确处理最高 20:1 的不平衡比，这意味着，最常用类的数据中包含的行数可以比最不常用类多 20 倍。

### <a name="identify-models-with-imbalanced-data"></a>识别包含不平衡数据的模型

由于分类算法通常是按准确度评估的，因此，检查模型的准确度评分可以很好地识别该模型是否受到不平衡数据的影响。 模型在预测某些类时的准确度是否确实很高或很低？

此外，自动化 ML 运行会自动生成以下图表，以帮助你了解模型分类的正确性，并识别可能受到不平衡数据影响的模型。

图表| 说明
---|---
[混淆矩阵](how-to-understand-automated-ml.md#confusion-matrix)| 根据数据的实际标签评估正确分类的标签。 
[精准率-召回率](how-to-understand-automated-ml.md#precision-recall-chart)| 根据发现的数据标签实例比评估正确的标签比 
[ROC 曲线](how-to-understand-automated-ml.md#roc)| 根据误报标签比评估正确的标签比。

### <a name="handle-imbalanced-data"></a>处理不平衡的数据 

以下技术是用于处理自动化 ML 外部的不平衡数据的附加选项。 

- 通过向上采样较小的类或向下采样较大的类，重新采样来均衡类的不平衡性。 这些方法需要具备处理和分析方面的专业知识。

- 使用性能指标来更好地处理不平衡的数据。 例如，F1 评分是精准率和召回率的加权平均值。 精准率度量分类器的精确性 -- 精准率较低表示存在大量误报；召回率度量分类器的完整性 - 召回率较低表示存在大量的漏报。 

## <a name="use-with-onnx-in-c-apps"></a>与 C# 应用中的 ONNX 配合使用

借助 Azure 机器学习，可以使用自动化 ML 来生成 Python 模型并将其转换为 ONNX 格式。 ONNX 运行时支持 C#，因此，可以在 C# 应用中使用自动生成的模型，而无需重新编写代码，同时可避免 REST 终结点造成的任何网络延迟。 在[此 Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) 中查看此流的示例。

## <a name="automated-ml-in-azure-machine-learning"></a>Azure 机器学习中的自动化 ML

Azure 机器学习提供了两种使用自动化 ML 的体验方式

* 对于编码经验丰富的客户，可以使用 [Azure 机器学习 Python SDK](https://docs.microsoft.com/python/api/overview/azureml-sdk/?view=azure-ml-py) 

* 对于编码经验有限或没有经验的客户，可以使用 [https://ml.azure.com](https://ml.azure.com/) 处的 Azure 机器学习工作室  

下面汇总了每种体验中支持的概要自动化 ML 功能。

<a name="parity"></a>

### <a name="experiment-settings"></a>试验设置 

以下设置允许你配置自动化 ML 试验。 

| | Python SDK| 工作室
----|:----:|:----:
将数据拆分为训练/验证集| ✓|✓
支持 ML 任务：分类、回归和预测| ✓| ✓
基于主要指标进行优化| ✓| ✓
支持将 AML 计算作为计算目标 | ✓|✓
配置预测范围、目标滞后和滚动窗口|✓|✓
设置退出条件 |✓|✓ 
设置并发迭代数| ✓|✓
删除列| ✓|✓
块算法|✓|✓
交叉验证 |✓|✓
支持在 Azure Databricks 群集上训练| ✓|
查看工程特征名称|✓|
特征化摘要| ✓|
假日特征化|✓|
日志文件的详细级别| ✓|

### <a name="model-settings"></a>模型设置

这些设置可作为自动化 ML 试验的结果应用于最佳模型。

||Python SDK|工作室
----|:----:|:----:
最佳模型注册| ✓|✓
最佳模型部署| ✓| ✓
最佳模型可说明性| ✓|✓
启用投票集成和堆栈集成模型| ✓|✓
显示基于非主要指标的最佳模型|✓|启用/禁用 ONNX 模型兼容性|✓|
测试模型 | ✓| |

### <a name="run-control-settings"></a>运行控制设置

通过这些设置，你可以查看和控制实验运行及其子运行。 

||Python SDK| 工作室
----|:----:|:----:
运行摘要表| ✓|✓
取消运行| ✓|✓
取消子运行| ✓| ✓
获取护栏| ✓|✓
暂停运行| ✓| 
恢复运行| ✓| 

## <a name="next-steps"></a>后续步骤

查看示例并了解如何使用自动化机器学习生成模型：

+ 遵循[教程：使用 Azure 机器学习自动训练回归模型](tutorial-auto-train-models.md)

+ 配置自动训练试验的设置：
  + 在 Azure 机器学习工作室中[使用这些步骤](how-to-create-portal-experiments.md)。
  + 在 Python SDK 中[使用这些步骤](how-to-configure-auto-train.md)。

+ 了解如何[使用这些步骤](how-to-auto-train-forecast.md)通过时序数据自动进行训练。

+ 查看[自动化机器学习的 Jupyter Notebook 示例](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)

* 自动化 ML 也可以在其他 Microsoft 解决方案中使用，例如 [ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)、[HDInsight](../hdinsight/spark/apache-spark-run-machine-learning-automl.md)、[Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated) 和 [SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)
