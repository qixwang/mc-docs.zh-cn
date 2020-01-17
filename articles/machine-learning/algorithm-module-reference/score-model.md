---
title: 评分模型：模块参考
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习中的“评分模型”模块使用训练分类或回归模型生成预测值。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 8e4bbc3324355b3413161639cf326120850829c7
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75598402"
---
# <a name="score-model-module"></a>“评分模型”模块

本文介绍 Azure 机器学习设计器（预览版）中的一个模块。

使用此模块可以通过经训练的分类或回归模型生成预测值。

## <a name="how-to-use"></a>如何使用

1. 将“评分模型”模块添加到管道  。

2. 连接经训练的模型和包含新输入数据的数据集。 

    数据应采用与所使用的经训练模型类型兼容的格式。 输入数据集的架构通常还应与用于训练模型的数据的架构相匹配。

3. 运行管道。

## <a name="results"></a>结果

使用[评分模型](./score-model.md)生成一组分数后：

+ 生成一组用于评估模型准确性（性能）的指标。  可以连接经评分的数据集以[评估模型](./evaluate-model.md)， 
+ 右键单击模块并选择“可视化”可查看结果的示例  。
+ 将结果保存到数据集。

分数（或预测值）可以采用多种不同的格式，具体取决于模型和输入数据：

- 对于分类模型，[分数模型](./score-model.md)输出类的预测值，以及预测值的概率。
- 对于回归模型，[评分模型](./score-model.md)仅生成预测数值。
- 对于图像分类模型，分数可以表示图像中的对象类，或指示是否找到了特定特征的布尔值。

## <a name="publish-scores-as-a-web-service"></a>将评分发布为 Web 服务

评分的一个常见用途是在预测 Web 服务中返回输出。 有关详细信息，请参阅本教程，了解如何在 Azure 机器学习中基于管道创建 Web 服务：

## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 