---
title: 转换为图像目录
titleSuffix: Azure Machine Learning
description: 了解如何使用“转换为图像目录”模块将数据集转换为图像目录格式。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: d03c54f7eade83d16f8b96c4e063178239005e5a
ms.sourcegitcommit: 1c01c98a2a42a7555d756569101a85e3245732fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85097786"
---
# <a name="convert-to-image-directory"></a>转换为图像目录

本文介绍如何使用“转换为图像目录”模块将图像数据集转换为“图像目录”数据类型，该类型采用图像相关任务（例如 Azure 机器学习设计器（预览版）中的图像分类）中的标准化数据格式。

## <a name="how-to-use-convert-to-image-directory"></a>如何使用“转换为图像目录”模块  

1.  将“转换为图像目录”**** 模块添加到试验中。 可以在模块列表的“计算机视觉/图像数据转换”类别中找到此模块。 

2.  将图像数据集作为输入进行连接。 请确保输入数据集中有图像。
    支持以下数据集格式：

    - 具有以下扩展名的压缩文件：“.zip”、“.tar”、“.gz”、“.bz2”。
    - 包含一个压缩文件（采用上述有效扩展名）的文件夹。 
    - 包含图像的文件夹。

    > [!NOTE]
    > 如果此图像数据集以 torchvision ImageFolder 格式进行组织，则可以在模块输出中记录图像类别。有关详细信息，请参阅 [torchvision 数据集](https://pytorch.org/docs/stable/torchvision/datasets.html#imagefolder)。 否则，只保存图像。

3.  提交管道。

## <a name="results"></a>结果

****“转换为图像目录”模块的输出为“图像目录”格式，可连接到其他与图像相关的模块（其输入端口格式也是“图像目录”）。

## <a name="technical-notes"></a>技术说明 

###  <a name="expected-inputs"></a>所需输入  

| 名称          | 类型                  | 说明   |
| ------------- | --------------------- | ------------- |
| 输入数据集 | AnyDirectory、ZipFile | 输入数据集 |

###  <a name="output"></a>输出  

| 名称                   | 类型           | 说明            |
| ---------------------- | -------------- | ---------------------- |
| 输出图像目录 | ImageDirectory | 输出图像目录 |

## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 
