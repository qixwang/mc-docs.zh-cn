---
title: 使用 Azure 机器学习设计器重新训练模型（预览版）
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习设计器（预览版）中发布的管道重新训练模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: peterlu
author: peterclu
ms.date: 12/15/2019
ms.openlocfilehash: de54c37554416cad7e6fcfeea3ac3b55ab613392
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75598128"
---
# <a name="retrain-models-with-azure-machine-learning-designer-preview"></a>使用 Azure 机器学习设计器重新训练模型（预览版）
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本操作方法指南介绍如何使用 Azure 机器学习设计器重新训练机器学习模型。 了解如何使用发布的管道自动执行机器学习工作流以进行重新训练。

本文介绍如何执行以下操作：

> [!div class="checklist"]
> * 训练机器学习模型。
> * 创建管道参数。
> * 发布训练管道。
> * 重新训练模型。

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果没有 Azure 订阅，请创建一个[免费帐户](https://aka.ms/AMLFree)。

* 具有企业版 SKU 的 Azure 机器学习工作区。

本操作方法假定你已具备在设计器中生成管道的基本知识。 有关设计器的引导式简介，请完成[教程](tutorial-designer-automobile-price-train-score.md)。 

### <a name="sample-pipeline"></a>示例管道

本文中使用的管道是[示例 3 - 收入预测](how-to-designer-sample-classification-predict-income.md)中所用管道的修改版。 它使用[导入数据](algorithm-module-reference/import-data.md)模块而非示例数据集来演示如何使用自己的数据训练模型。

![屏幕截图显示修改后的示例管道，其中使用方框突出显示了“导入数据”模块](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="train-a-machine-learning-model"></a>训练机器学习模型

若要重新训练模型，需要一个初始模型。 本部分介绍如何使用设计器训练模型和访问已保存的模型。

1. 选择“导入数据”  模块。
1. 在“属性”窗格中，指定数据源。

    ![屏幕截图显示“导入数据”模块的示例配置](./media/how-to-retrain-designer/import-data-settings.png)

    在此示例中，数据存储在 [Azure 数据存储](how-to-access-data.md)中。 如果还没有数据存储，可以通过选择“新建数据存储”  来创建一个数据存储。

1. 指定数据路径。 还可以选择“浏览路径”  以直观方式浏览数据存储。 

1. 选择画布顶部的“运行”  以运行管道。
    
    > [!NOTE]
    > 如果已为此管道草稿设置了默认计算，管道会自动运行。 如果没有，可以按照“设置”窗格中显示的提示进行操作，现在设置一个。

### <a name="locate-your-trained-model"></a>找到已训练的模型

设计器会将所有管道输出（包括已训练的模型）保存到默认存储帐户中。 还可以直接在设计器中访问已训练的模型：

1. 等待管道完成运行。

1. 选择**训练模型**模块。

1. 在“设置”窗格中，选择“输出”  。

1. 选择“Trained_model”  以下载模型。

![屏幕截图显示如何下载已训练的模型](./media/how-to-retrain-designer/download-model.png)

## <a name="create-a-pipeline-parameter"></a>创建管道参数

可以添加管道参数，在运行时动态设置变量。 针对此管道，为训练数据路径添加管道参数，以便可以在新数据集上重新训练模型。

1. 选择“导入数据”  模块。
1. 在“设置”窗格中，选择“路径”  字段上方的省略号。
1. 选择“添加到管道参数”  。
1. 提供参数名称和默认值。

    > [!NOTE]
    > 可以通过选择管道草稿标题旁的“设置”齿轮图标  来检查和编辑管道参数。 

[屏幕截图显示如何创建管道参数](media/how-to-retrain-designer/add-pipeline-parameter.png)

## <a name="publish-a-training-pipeline"></a>发布训练管道

发布管道后，系统会创建管道终结点。 通过管道终结点，可重复使用和管理管道，实现可重复性和自动化。 为实现此方案，应发布训练管道，重复使用它进行重新训练。

1. 选择设计器画布上方的“发布”  。
1. 选择或创建一个新的管道终结点。

    > [!NOTE]
    > 可将多个管道发布到一个终结点。 终结点中的每个管道都会得到一个版本号，可以在调用管道终结点时指定该版本号。

1. 选择“发布”  。

## <a name="retrain-your-model"></a>重新训练模型

现在，你已有了一个发布的训练管道，可以使用它和新数据来重新训练模型。 可以通过门户或以编程方式从管道终结点提交运行。

### <a name="submit-runs-with-the-designer"></a>用设计器提交运行

通过以下步骤从设计器提交管道终结点运行：

1. 转到“终结点”  页。

1. 选择“管道终结点”  选项卡。

1. 选择管道终结点。

1. 选择“已发布的管道”  选项卡。

1. 选择要运行的管道。

1. 选择“运行”。 

1. 在“设置”对话框中，可以指定新的输入数据路径值，该值指向新的数据集。

![屏幕截图显示如何在设计器中设置参数化管道运行](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-with-code"></a>通过代码提交运行

可以通过多种方法以编程方式访问 REST 终结点，具体取决于开发环境。 可以在管道的“使用”  选项卡中找到说明如何使用参数提交管道运行的代码示例。

## <a name="next-steps"></a>后续步骤

按照设计器[教程](tutorial-designer-automobile-price-train-score.md)训练和部署回归模型。