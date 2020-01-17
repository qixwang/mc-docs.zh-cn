---
title: include 文件
description: include 文件
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 12/04/2019
ms.openlocfilehash: 58e19997d659b7c19c26a7af5eaac339b578103d
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75598075"
---
**计算目标的重复使用可以从一个训练作业延续到下一个训练作业**。 例如，将远程 VM 附加到你的工作区后，可以将其重复用于多个作业。  对于机器学习管道，请为每个计算目标使用适当的[管道步骤](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py)。

|训练&nbsp;目标|[自动化机器学习](../articles/machine-learning/concept-automated-ml.md) | [机器学习管道](../articles/machine-learning/concept-ml-pipelines.md) | [Azure 机器学习设计器](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[本地计算机](../articles/machine-learning/how-to-set-up-training-targets.md#local)| 是 | &nbsp; | &nbsp; |
|[Azure 机器学习计算实例（预览）](../articles/machine-learning/concept-compute-instance.md) | | 是 |  |
|[Azure 机器学习计算群集](../articles/machine-learning/how-to-set-up-training-targets.md#amlcompute)| 是 & <br/>超参数&nbsp;优化 | 是 | 是 |
|[远程 VM](../articles/machine-learning/how-to-set-up-training-targets.md#vm) | 是 & <br/>超参数优化 | 是 | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/how-to-create-your-first-pipeline.md#databricks)| 是 | 是 | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-create-your-first-pipeline.md#adla) | &nbsp; | 是 | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-set-up-training-targets.md#hdinsight) | &nbsp; | 是 | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-set-up-training-targets.md#azbatch) | &nbsp; | 是 | &nbsp; |
