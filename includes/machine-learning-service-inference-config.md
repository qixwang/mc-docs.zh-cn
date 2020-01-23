---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 11/06/2019
ms.author: larryfr
ms.openlocfilehash: 3fc76779e92024b0082ac4013a68d6de5bc0267f
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75599408"
---
`inferenceconfig.json` 文档中的条目映射到 [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) 类的参数。 下表描述了 JSON 文档中的实体与方法参数之间的映射：

| JSON 实体 | 方法参数 | 说明 |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | 包含要为映像运行的代码的本地文件路径。 |
| `runtime` | `runtime` | 可选。 用于映像的运行时。 当前支持的运行时为 `spark-py` 和 `python`。 如果设置 `environment`，则将忽略此项。 |
| `condaFile` | `conda_file` | 可选。 包含要用于映像的 Conda 环境定义的本地文件路径。  如果设置 `environment`，则将忽略此项。 |
| `extraDockerFileSteps` | `extra_docker_file_steps` | 可选。 包含设置映像时要运行的附加 Docker 步骤的本地文件路径。  如果设置 `environment`，则将忽略此项。|
| `sourceDirectory` | `source_directory` | 可选。 包含要创建映像的所有文件的文件夹路径。 |
| `enableGpu` | `enable_gpu` | 可选。 是否在映像中启用 GPU 支持。 GPU 映像必须在 Azure 服务上运行，如 Azure 容器实例、Azure 机器学习计算、Azure 虚拟机和 Azure Kubernetes 服务。 默认为 False。 如果设置 `environment`，则将忽略此项。|
| `baseImage` | `base_image` | 可选。 要用作基础映像的自定义映像。 如果未提供基础映像，则该映像将基于提供的运行时参数。 如果设置 `environment`，则将忽略此项。 |
| `baseImageRegistry` | `base_image_registry` | 可选。 包含基础映像的映像注册表。 如果设置 `environment`，则将忽略此项。|
| `cudaVersion` | `cuda_version` | 可选。 要为需要 GPU 支持的映像安装的 CUDA 版本。 GPU 映像必须在 Azure 服务上运行，如 Azure 容器实例、Azure 机器学习计算、Azure 虚拟机和 Azure Kubernetes 服务。 支持的版本为 9.0、9.1 和 10.0。 如果设置 `enable_gpu`，则默认为 9.1。 如果设置 `environment`，则将忽略此项。 |
| `description` | `description` | 映像的说明。 如果设置 `environment`，则将忽略此项。  |
| `environment` | `environment` | 可选。  Azure 机器学习[环境](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)。|

以下 JSON 是用于 CLI 的推理配置示例：

```json
{
    "entryScript": "score.py",
    "runtime": "python",
    "condaFile": "myenv.yml",
    "extraDockerfileSteps": null,
    "sourceDirectory": null,
    "enableGpu": false,
    "baseImage": null,
    "baseImageRegistry": null
}
```

以下 JSON 是用于 CLI 的推理配置示例，其中使用了现有特定版本的 Azure 机器学习[环境](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)：

```json
{
    "entryScript": "score.py",
    "environment":{
        "name": "myenv",
        "version": "1"
    },
    "condaFile": "myenv.yml",
    "sourceDirectory": null
}
```

以下 JSON 是用于 CLI 的推理配置示例，其中使用了现有最新版本的 Azure 机器学习[环境](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)：

```json
{
    "entryScript": "score.py",
    "environment":{
        "name": "myenv",
        "version": null
    },
    "condaFile": "myenv.yml",
    "sourceDirectory": null
}
```
