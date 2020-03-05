---
title: Azure 机器学习的 Git 集成
titleSuffix: Azure Machine Learning
description: 了解 Azure 机器学习如何与本地 Git 存储库集成。 当从本地目录（Git 存储库）提交训练运行时，系统会将有关存储库、分支和当前提交的信息作为运行的一部分进行跟踪。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: v-yiso
author: jpe316
origin.date: 10/11/2019
ms.date: 03/09/2020
ms.openlocfilehash: 4a9ce9bc5743e600d54bca576bbe934810af3906
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78154658"
---
# <a name="git-integration-for-azure-machine-learning"></a>Azure 机器学习的 Git 集成

[Git](https://git-scm.com/) 是一种常用的版本控制系统，可用于共享和协作处理项目。 在向 Azure 机器学习提交训练作业时，如果训练文件存储在本地 git 存储库中，那么系统会将有关存储库的信息作为训练过程的一部分进行跟踪。

由于 Azure 机器学习会跟踪来自本地 git 存储库的信息，因此它不会绑定到任何特定的中心存储库。 可以从 GitHub、GitLab、Bitbucket、Azure DevOps 或任何其他与 git 兼容的服务克隆存储库。

## <a name="how-does-git-integration-work"></a>Git 集成是如何工作的？

从 Python SDK 或机器学习 CLI 提交训练运行时，训练模型所需的文件将上传到工作区。 如果可在开发环境中使用 `git` 命令，则上传过程会使用该命令检查文件是否存储在 git 存储库中。 如果是，那么 git 存储库中的信息也会作为训练运行的一部分上传。 此信息存储在训练运行的以下属性中：

| 属性 | 用于获取值的 Git 命令 | 说明 |
| ----- | ----- | ----- |
| `azureml.git.repository_uri` | `git ls-remote --get-url` | 从中克隆存储库的 URI。 |
| `mlflow.source.git.repoURL` | `git ls-remote --get-url` | 从中克隆存储库的 URI。 |
| `azureml.git.branch` | `git symbolic-ref --short HEAD` | 提交运行时的活动分支。 |
| `mlflow.source.git.branch` | `git symbolic-ref --short HEAD` | 提交运行时的活动分支。 |
| `azureml.git.commit` | `git rev-parse HEAD` | 为运行提交的代码的提交哈希。 |
| `mlflow.source.git.commit` | `git rev-parse HEAD` | 为运行提交的代码的提交哈希。 |
| `azureml.git.dirty` | `git status --porcelain .` | 如果分支/提交异常，则为 `True`；否则为 `false`。 |

对于使用估计器、机器学习管道或脚本运行的运行，将发送此信息。

如果训练文件不在开发环境的 git 存储库中，或者 `git` 命令不可用，则不会跟踪与 git 相关的信息。

> [!TIP]
> 若要检查 git 命令在开发环境中是否可用，请打开 shell 会话、命令提示符、PowerShell 或其他命令行接口，并键入以下命令：
>
> ```
> git --version
> ```
>
> 如果已安装且在路径中，则会收到类似于 `git version 2.4.1` 的响应。 有关在开发环境中安装 git 的详细信息，请参阅 [Git 网站](https://git-scm.com/)。

## <a name="view-the-logged-information"></a>查看记录的信息

Git 信息存储在训练运行的属性中。 可以使用 Azure 门户、Python SDK 和 CLI 查看此信息。 

### <a name="azure-portal"></a>Azure 门户

1. 在 [Azure 门户](https://portal.azure.cn)中，选择工作区。
1. 选择“试验”，然后选择一个试验  。
1. 从“运行号”列中选择一个运行  。
1. 选择“日志”，然后展开“日志”和“azureml”条目    。 选择以“###\_azure”开头的链接  。

    ![门户中的 ###_azure 条目](./media/concept-train-model-git-integration/azure-machine-learning-logs.png)

记录的信息包含类似于以下 JSON 的文本：

```json
"properties": {
    "_azureml.ComputeTargetType": "batchai",
    "ContentSnapshotId": "5ca66406-cbac-4d7d-bc95-f5a51dd3e57e",
    "azureml.git.repository_uri": "git@github.com:azure/machinelearningnotebooks",
    "mlflow.source.git.repoURL": "git@github.com:azure/machinelearningnotebooks",
    "azureml.git.branch": "master",
    "mlflow.source.git.branch": "master",
    "azureml.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "mlflow.source.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "azureml.git.dirty": "True",
    "AzureML.DerivedImageName": "azureml/azureml_9d3568242c6bfef9631879915768deaf",
    "ProcessInfoFile": "azureml-logs/process_info.json",
    "ProcessStatusFile": "azureml-logs/process_status.json"
}
```

### <a name="python-sdk"></a>Python SDK

提交训练运行后，将返回 [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) 对象。 此对象的 `properties` 属性包含记录的 git 信息。 例如，以下代码可检索提交哈希：

```python
run.properties['azureml.git.commit']
```

### <a name="cli"></a>CLI

`az ml run` CLI命令可用于从运行中检索属性。 例如，以下命令返回名为 `train-on-amlcompute` 的试验中最后一次运行的属性：

```azurecli
az ml run list -e train-on-amlcompute --last 1 -w myworkspace -g myresourcegroup --query '[].properties'
```

有关详细信息，请参阅 [az ml run](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest) 参考文档。

## <a name="next-steps"></a>后续步骤

* [设置并使用模型训练的计算目标](how-to-set-up-training-targets.md)