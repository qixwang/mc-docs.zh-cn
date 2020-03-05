---
title: 应用转换：模块参考
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习中的“应用转换”模块来修改基于之前计算的转换的输入数据集。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: v-yiso
ms.date: 03/09/2020
origin.date: 02/11/2020
ms.openlocfilehash: d4d616962d461f5fc641dcb9ccd5ea3c23bc8628
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78154952"
---
# <a name="apply-transformation-module"></a>“应用转换”模块

本文介绍 Azure 机器学习设计器（预览版）中的模块。

使用此模块来修改基于之前计算的转换的输入数据集。  
  
例如，如果使用了 z 分数通过“规范化数据”模块来规范化训练数据，则可能还需要使用在评分阶段为训练计算的 z 分数值。  在 Azure 机器学习中，可以将规范化方法另存为转换，然后使用“应用转换”在评分之前将 z 分数应用于输入数据。 
  
Azure 机器学习为创建和应用多种不同类型的自定义转换提供支持。 例如，你可能想要保存并重复使用转换来执行以下操作：  
  
- 使用“清理缺失数据”来删除或替换缺失值 
- 使用“规范化数据”来规范化数据 
  

## <a name="how-to-use-apply-transformation"></a>如何使用“应用转换”  
  
1. 将“应用转换”  模块添加到管道。 可以在“模型评分和评估”类别中找到此模块  。 
  
2. 找到要用作输入的现有转换。 以前保存的转换可在左侧模块树中“数据集”  类别下的“我的数据集”  组中找到。  
  
   
  
3. 连接要转换的数据集。 数据集应具有与首次为其设计了转换的数据集相同的架构（列数、列名、数据类型）。  
  
4. 不需要设置其他参数，因为在定义转换时已完成所有自定义。  
  
5. 若要将转换应用于新数据集，请运行管道。  

## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 