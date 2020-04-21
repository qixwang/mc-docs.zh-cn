---
title: 使用 Azure 机器学习设计器重新训练模型（预览版）
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习设计器（预览版）中发布的管道重新训练模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: v-yiso
author: likebupt
origin.date: 02/24/2020
ms.date: 03/16/2020
ms.openlocfilehash: 2e22b96d3cd3449525eb720c7cbc4d664e242f7e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80343383"
---
# <a name="retrain-models-with-azure-machine-learning-designer-preview"></a>使用 Azure 机器学习设计器重新训练模型（预览版）
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

本操作方法文章介绍如何使用 Azure 机器学习设计器重新训练机器学习模型。 了解如何使用发布的管道自动执行机器学习工作流以进行重新训练。

在本文中，学习如何：

> [!div class="checklist"]
> * 训练机器学习模型。
> * 创建管道参数。
> * 发布训练管道。
> * 重新训练模型。

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果还没有 Azure 订阅，可以创建一个[免费帐户](https://aka.ms/AMLFree)。
* 具有企业版 SKU 的 Azure 机器学习工作区。

本文假定你已具备在设计器中生成管道的基本知识。 有关设计器的引导式简介，请完成[教程](tutorial-designer-automobile-price-train-score.md)。 

### <a name="sample-pipeline"></a>示例管道

本文中使用的管道是[示例 3：收入预测](how-to-designer-sample-classification-predict-income.md)中所用管道的修改版。 它使用[导入数据](algorithm-module-reference/import-data.md)模块而非示例数据集来演示如何使用你自己的数据来训练模型。

![屏幕截图显示了修改后的示例管道，其中使用方框突出显示了“导入数据”模块](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="train-a-machine-learning-model"></a>训练机器学习模型

若要重新训练模型，需要一个初始模型。 本部分介绍了如何使用设计器来训练模型以及访问已保存的模型。

1. 选择“导入数据”  模块。
1. 在“属性”窗格中，指定数据源。

   ![屏幕截图显示了“导入数据”模块的示例配置](./media/how-to-retrain-designer/import-data-settings.png)

   在此示例中，数据存储在 [Azure 数据存储](how-to-access-data.md)中。 如果还没有数据存储，可以通过选择“新建数据存储”  来创建一个数据存储。

1. 指定数据路径。 还可以选择“浏览路径”  来浏览找到你的数据存储。 

1. 在画布顶部选择“运行”。 
    
   > [!NOTE]
   > 如果已为此管道草稿设置了默认计算，管道会自动运行。 如果没有，可以按照“设置”窗格中的提示进行操作，现在设置一个。

### <a name="find-your-trained-model"></a>查找已训练的模型

设计器会将所有管道输出（包括已训练的模型）保存到默认存储帐户中。 还可以直接在设计器中访问已训练的模型：

1. 等待管道完成运行。
1. 选择**训练模型**模块。
1. 在“设置”窗格中，选择“输出+日志”  。
1. 选择“查看输出”  图标，然后按照弹出窗口中的说明找到已训练的模型。

![屏幕截图显示了如何下载已训练的模型](./media/how-to-retrain-designer/trained-model-view-output.png)

## <a name="create-a-pipeline-parameter"></a>创建管道参数

添加管道参数，以便在运行时动态设置变量。 对于此管道，为训练数据路径添加参数，以便在新数据集上重新训练模型。

1. 选择“导入数据”  模块。
1. 在“设置”窗格中，选择“路径”  字段上方的省略号。
1. 选择“添加到管道参数”  。
1. 提供参数名称和默认值。

   > [!NOTE]
   > 可以通过选择管道草稿标题旁的“设置”齿轮图标  来检查和编辑管道参数。 

![屏幕截图显示了如何创建管道参数](media/how-to-retrain-designer/add-pipeline-parameter.png)

## <a name="publish-a-training-pipeline"></a>发布训练管道

发布管道后，系统会创建管道终结点。 通过管道终结点，可重复使用和管理管道，实现可重复性和自动化。 在此示例中，你已设置了管道以便重新训练。

1. 选择设计器画布上方的“发布”  。
1. 选择或创建一个管道终结点。

    > [!NOTE]
    > 可将多个管道发布到一个终结点。 终结点中的每个管道都会得到一个版本号，可以在调用管道终结点时指定该版本号。

1. 选择“发布”  。

## <a name="retrain-your-model"></a>重新训练模型

现在，你已有了一个发布的训练管道，可以使用它和新数据来重新训练模型。 可以通过 Azure 门户从管道终结点提交运行，也可以采用编程方式提交它们。

### <a name="submit-runs-by-using-the-designer"></a>使用设计器提交运行

通过以下步骤从设计器提交管道终结点运行：

1. 转到“终结点”  页。
1. 选择“管道终结点”  选项卡。
1. 选择管道终结点。
1. 选择“已发布的管道”  选项卡。
1. 选择要运行的管道。
1. 选择“提交”。 
1. 在“设置”对话框中，你可以为输入数据路径值指定一个新值。 此值指向你的新数据集。

![屏幕截图显示了如何在设计器中设置参数化管道运行](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-by-using-code"></a>使用代码提交运行

可以在概述面板中找到已发布管道的 REST 终结点。 通过调用该终结点，你可以重新训练已发布的管道。

若要发出 REST 调用，你需要 OAuth 2.0 持有者类型身份验证标头。 有关为你的工作区设置身份验证以及进行参数化 REST 调用的信息，请参阅[生成用于批量评分的 Azure 机器学习管道](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint)。

## <a name="next-steps"></a>后续步骤

按照[设计器教程](tutorial-designer-automobile-price-train-score.md)来训练和部署回归模型。
