---
title: Azure 机器学习中的数据
titleSuffix: Azure Machine Learning
description: 了解 Azure 机器学习如何与数据交互，以及如何在不同的机器学习试验中利用数据。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 03/15/2020
ms.openlocfilehash: 8201fe105d2b8a166bf09821c12949bf6834be15
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80343570"
---
# <a name="data-access-in-azure-machine-learning"></a>Azure 机器学习中的数据访问

本文介绍 Azure 机器学习的数据管理，以及机器学习任务的集成解决方案。 本文假设已创建一个 [Azure 存储帐户](/storage/common/storage-quickstart-create-account?tabs=azure-portal)和 [Azure 存储服务](/storage/common/storage-introduction)。

准备好使用 Azure 存储解决方案中的数据时，我们建议你

1. 创建一个 Azure 机器学习数据存储。
2. 在该数据存储中创建一个 Azure 机器学习数据集。 
3. 通过以下任一方式在机器学习试验中使用该数据集： 
    1. 将该数据集装载到试验的计算目标以进行模型训练

        **OR** 

    1. 在自动化机器学习（自动化 ML）试验运行、机器学习管道和 [Azure 机器学习设计器](concept-designer.md)等 Azure 机器学习解决方案中直接使用该数据集。
4. 针对模型输出数据集创建数据集监视器以检测数据偏移。 
5. 如果检测到数据偏移，请更新输入数据集，并相应地重新训练模型。

下图提供了建议的这种数据访问工作流的直观演示。

![Data-concept-diagram](./media/concept-data/data-concept-diagram.svg)

## <a name="access-data-in-storage"></a>访问存储中的数据

Azure 机器学习提供数据存储和数据集用于访问存储帐户中的数据。 数据存储解答以下问题：如何安全连接到 Azure 存储中的数据？ 数据存储会将连接信息保存到 Azure 存储。 这有助于提高安全性并方便访问存储，因为连接信息保存在数据存储中，而不会在脚本中公开。 

数据集解答以下问题：如何获取数据存储中的特定数据文件？ 数据集指向底层存储中用于机器学习试验的一个或多个特定文件。 数据存储和数据集共同为机器学习任务提供安全、可缩放且可再现的数据交付工作流。

## <a name="datastores"></a>数据存储

Azure 机器学习数据存储会将连接信息保存到你的存储中，因此你不必在脚本中对其进行编码。 [注册并创建一个数据存储](how-to-access-data.md)即可轻松连接到 Azure 存储帐户，并访问底层 Azure 存储服务中的数据。

可注册为数据存储的受支持 Azure 存储服务：
+ Azure Blob 容器
+ Azure 文件共享
+ Azure Data Lake
+ Azure Data Lake Gen2
+ Azure SQL 数据库
+ Azure Database for PostgreSQL
+ Databricks 文件系统
+ Azure Database for MySQL

## <a name="datasets"></a>数据集

[创建 Azure 机器学习数据集](how-to-create-register-datasets.md)可与数据存储中的数据进行交互，并将数据打包成机器学习任务可用的对象。 将数据集注册到工作区可在不同的试验中共享和重用该数据集，而不会造成数据引入复杂性。

可以从本地文件、公共 URL、Azure 开放数据集或数据存储中的特定文件创建数据集。 若要从内存中 pandas 数据帧创建数据集，请将数据写入本地文件（例如 csv），然后从该文件创建数据集。 数据集不是数据的副本，而是指向存储服务中的数据的引用，因此不会产生额外的存储费用。 

下图显示，如果没有 Azure 存储服务，可以直接从本地文件、公共 URL 或 Azure 开放数据集创建数据集。 这样就可以将数据集连接到使用试验的 [Azure 机器学习工作区](concept-workspace.md)自动创建的默认数据存储。

![Data-concept-diagram](./media/concept-data/dataset-workflow.svg)

可在以下文档中找到更多数据集功能：

+ 对数据集世系进行[版本控制和跟踪](how-to-version-track-datasets.md)。
+ [监视数据集](how-to-monitor-datasets.md)以帮助检测数据偏移。
+  参阅有关以下两种类型的数据集的文档：
    + [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) 表示表格格式的、通过分析提供的文件或文件列表所创建的数据。 它可以让你将数据具体化为 Pandas 或 Spark 数据帧，以进一步进行处理和清理。 有关可从中创建 TabularDataset 的文件的完整列表，请参阅 [TabularDatasetFactory 类](https://aka.ms/tabulardataset-api-reference)。

    + [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) 引用数据存储或公共 URL 中的单个或多个文件。 通过此方法可将所选的文件作为 FileDataset 对象下载或装载到计算目标。

## <a name="work-with-your-data"></a>使用数据

使用数据集可以通过与 Azure 机器学习功能的无缝集成来完成许多机器学习任务。 

+ 创建[数据标记项目](#label)。
+ 从 [Azure 开放数据集](how-to-create-register-datasets.md#create-datasets-with-azure-open-datasets)创建数据集。
+ [训练机器学习模型](how-to-train-with-datasets.md)。
+ 在以下位置使用数据集 
     + [自动化 ML 试验](how-to-use-automated-ml-for-ml-models.md)
     + [设计器](tutorial-designer-automobile-price-train-score.md#import-data) 
     + [Azure 机器学习管道](how-to-create-your-first-pipeline.md)
+ 访问数据集，以使用[机器学习管道](how-to-create-your-first-pipeline.md)中的批量推理进行评分。
+ 设置数据集监视器以检测[数据偏移](#drift)。

<a name="label"></a>

## <a name="data-labeling"></a>数据标记

在机器学习项目中，标记大量数据通常是一件很麻烦的事情。 这些包含计算机视觉组件（如图像分类或对象检测）的项目通常需要数千个图像和对应的标签。

Azure 机器学习提供了一个中心位置，用于创建、管理和监视标签项目。 标记项目有助于协调数据、标签和团队成员，使你能够更有效地管理标记任务。 当前支持的任务包括图像分类（多标签或多类）以及使用边界框的对象标识。

+ 创建[数据标记项目](how-to-create-labeling-projects.md)，并输出可在机器学习试验中使用的数据集。

<a name="drift"></a>

## <a name="data-drift"></a>数据偏移

在机器学习的上下文中，数据偏移是指模型输入数据中导致模型性能下降的变化。 这是模型准确度在一段时间后下降的最主要原因之一，因此，监视数据偏移有助于检测模型性能问题。
请参阅[创建数据集监视器](how-to-monitor-datasets.md)一文来详细了解如何检测数据集中新数据的数据偏移并发出警报。

## <a name="next-steps"></a>后续步骤 

+ 若要在 Azure 机器学习工作室或 Python SDK 中创建数据集，请[使用这些步骤](how-to-create-register-datasets.md)。
+ 阅读[示例笔记本](https://aka.ms/dataset-tutorial)中的数据集训练示例。
+ 有关数据偏移的示例，请参阅此[数据偏移教程](https://aka.ms/datadrift-notebook)。
