---
title: 设计器：预测航班延误示例
titleSuffix: Azure Machine Learning
description: 通过 Azure 机器学习设计器，生成分类器并使用自定义 R 代码预测航班延误。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: peterlu
ms.date: 11/04/2019
ms.openlocfilehash: 9eb43eee71ac361605624f52d4d4aab0ef7a40d0
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75597999"
---
# <a name="build-a-classifier--use-r-to-predict-flight-delays-with-azure-machine-learning-designer"></a>通过 Azure 机器学习设计器生成分类器并使用 R 预测航班延误

**设计器（预览）示例 6**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

此管道使用历史航班和天气数据来预测计划的客运航班是否会延迟 15 分钟以上。 此问题可作为分类问题来解决，预测两个类：延迟或准点。

下面是此示例的最终管道图：

[![管道图](media/how-to-designer-sample-classification-flight-delay/pipeline-graph.png)](media/how-to-designer-sample-classification-flight-delay/pipeline-graph.png#lightbox)

## <a name="prerequisites"></a>先决条件

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. 单击示例 6 将其打开。

## <a name="get-the-data"></a>获取数据

此示例使用“航班延误数据”数据集  。 它是美国交通部 TranStats 数据集合的一部分。 该数据集包含 2013 年四月到十月的航班延误信息。 该数据集已进行如下预处理：

* 筛选为包括美国大陆上最繁荣的 70 个机场。
* 将已取消的航班重新标记为延误超过 15 分钟。
* 筛选掉已改道的航班。
* 选出 14 列。

若要补充航班数据，请使用天气数据集  。 天气数据包含来自 NOAA 的每小时陆地天气观测结果，代表来自机场气象站的观测结果，涵盖与航班数据集相同的时间段。 它已经过如下预处理：

* 将气象站 ID 映射到相应的机场 ID。
* 删除了与最繁荣的 70 个的机场无关的气象站。
* 将“Date”列拆分为以下单独的列：“Year”、“Month”和“Day”。
* 选出 26 列。

## <a name="pre-process-the-data"></a>预处理数据

数据集通常需要经过一定的预处理才能进行分析。

![数据处理](./media/how-to-designer-sample-classification-flight-delay/data-process.png)

### <a name="flight-data"></a>航班数据

“Carrier”、“OriginAirportID”和“DestAirportID”列保存为整数    。 然而，它们都是分类特性，使用“编辑元数据”模块将其转换为分类  。

![编辑元数据](./media/how-to-designer-sample-classification-flight-delay/edit-metadata.png)

然后，在“数据集”模块中使用“选择列”，从数据集列中排除透露可能的目标的列  ：“DepDelay”、“DepDel15”、“ArrDelay”、“Canceled”、“Year”      。 

若要将航班记录与每小时天气记录联接，请使用计划的出发时间作为其中一个联接键。 若要执行联接操作，必须将“CSRDepTime”列去尾到最接近的整点，此操作可在“执行 R 脚本”模块中完成  。 

### <a name="weather-data"></a>天气数据

使用“项目列”模块排除具有大量缺失值的列  。 这些列包含所有字符串值列：“ValueForWindCharacter”、“WetBulbFarenheit”、“WetBulbCelsius”、“PressureTendency”、“PressureChange”、“SeaLevelPressure”和“StationPressure”        。

然后，将“清理缺失数据”模块应用到其余列以删除缺失数据的行  。

天气观察时间将向上舍入到最接近的完整小时。 计划的航班时间和天气观测时间以相反方向舍入，以确保模型只使用航班时间之前的天气。 

由于天气数据是以本地时间报告的，因此，通过从计划的出发时间和天气观测时间中减去时区列来计入时区差异。 使用“执行 R 脚本”模块完成这些操作  。

### <a name="joining-datasets"></a>联接数据集

使用“联接数据”模块，将航班记录与航班出发地（“OriginAirportID”）的天气数据相联接   。

 ![按出发地联接航班和天气](./media/how-to-designer-sample-classification-flight-delay/join-origin.png)


使用航班的目的地（“DestAirportID”）将航班记录与天气数据相联接  。

 ![按目的地联接航班和天气](./media/how-to-designer-sample-classification-flight-delay/join-destination.png)

### <a name="preparing-training-and-test-samples"></a>准备训练和测试示例

“拆分数据”模块将数据拆分为四月到九月的记录（用于训练）以及十月的记录（用于测试）  。

 ![拆分训练和测试数据](./media/how-to-designer-sample-classification-flight-delay/split.png)

使用“选择列”模块从训练数据集中删除“Year”、“Month”和“Timezone”列。

## <a name="define-features"></a>定义特征

在机器学习中，特征是你感兴趣的内容的各种可测量属性。 需要利用领域相关知识并进行试验才能找到一组健全的特征。 有些特征比其他特征更适合用于预测目标。 此外，某些特征可能与其他特征有很强的关联，不会为模型带来新信息。 可以删除这些特征。

构建模型时，可使用所有可用特征，或选择一部分特征。

## <a name="choose-and-apply-a-learning-algorithm"></a>选择并应用学习算法

使用“双类逻辑回归”模块创建一个模型，并使用训练数据集对其进行训练  。 

通过“训练模型”模块，可以得到经训练的分类模型，可用来为新样本评分以进行预测  。 使用测试集，由已训练的模型生成分数。 然后，使用“评估模型”模块来分析和比较模型的质量  。
运行管道后，可单击输出端口并选择“可视化”来查看“评分模型”模块的输出   。 输出包括评分的标签和标签的概率。

最后，要测试结果的质量，请将“评估模型”模块添加到管道画布，然后将左侧的输入端口连接到“评分模型”模块的输出  。 单击输出端口并选择“可视化”来运行管道并查看“评估模型”模块的输出   。

## <a name="evaluate"></a>评估
在测试集上，逻辑回归模型的 AUC 为 0.631。

 ![评估](./media/how-to-designer-sample-classification-flight-delay/evaluate.png)

## <a name="next-steps"></a>后续步骤

浏览可用于设计器的其他示例：

- [示例 1 - 回归：预测汽车的价格](how-to-designer-sample-regression-automobile-price-basic.md)
- [示例 2 - 回归：比较汽车价格预测的算法](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [示例 3 - 通过特征选择进行分类：收入预测](how-to-designer-sample-classification-predict-income.md)
- [示例 4 - 分类：预测信用风险（成本敏感）](service/how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [示例 5 - 分类：预测变动率](service/how-to-designer-sample-classification-churn.md)
- [示例 7 - 文本分类：维基百科 SP 500 数据集](how-to-designer-sample-text-classification.md)