---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 11/06/2019
ms.author: larryfr
ms.openlocfilehash: c3d39c2cdf52464306786eb3e5fded41a5eb1ce8
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78849891"
---
`inferenceconfig.json` 文档中的条目映射到 [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) 类的参数。 下表描述了 JSON 文档中的实体与方法参数之间的映射：

| JSON 实体 | 方法参数 | 说明 |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | 包含要为映像运行的代码的本地文件路径。 |
| `runtime` | `runtime` | 可选。 用于映像的运行时。 支持的运行时为 `spark-py` 和 `python`。 如果设置 `environment`，则会忽略此项。 |
| `condaFile` | `conda_file` | 可选。 包含要用于映像的 Conda 环境定义的本地文件路径。  如果设置 `environment`，则会忽略此项。 |
| `extraDockerFileSteps` | `extra_docker_file_steps` | 可选。 包含设置映像时要运行的附加 Docker 步骤的本地文件路径。  如果设置 `environment`，则会忽略此项。|
| `sourceDirectory` | `source_directory` | 可选。 包含用于创建映像的所有文件的文件夹路径，因此可以轻松地访问此文件夹或子文件夹中的任何文件。 可以从本地计算机上传整个文件夹，作为 Webservice 的依赖项。 注意：entry_script、conda_file 和 extra_docker_file_steps 路径是 source_directory 路径的相对路径。 |
| `enableGpu` | `enable_gpu` | 可选。 是否在映像中启用 GPU 支持。 GPU 映像必须在 Azure 服务上运行，如 Azure 容器实例。 例如，Azure 机器学习计算、Azure 虚拟机和 Azure Kubernetes 服务。 默认值为 False。 如果设置 `environment`，则会忽略此项。|
| `baseImage` | `base_image` | 可选。 要用作基础映像的自定义映像。 如果未提供基础映像，则该映像将基于提供的运行时参数。 如果设置 `environment`，则会忽略此项。 |
| `baseImageRegistry` | `base_image_registry` | 可选。 包含基础映像的映像注册表。 如果设置 `environment`，则会忽略此项。|
| `cudaVersion` | `cuda_version` | 可选。 要为需要 GPU 支持的映像安装的 CUDA 版本。 必须在 Azure 服务上使用 GPU 映像。 例如，Azure 容器实例、Azure 机器学习计算、Azure 虚拟机和 Azure Kubernetes 服务。 支持的版本为 9.0、9.1 和 10.0。 如果设置 `enable_gpu`，则默认为 9.1。 如果设置 `environment`，则会忽略此项。 |
| `description` | `description` | 映像的说明。 如果设置 `environment`，则会忽略此项。  |
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

可以在推理配置文件中包含 Azure 机器学习[环境](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)的完整规范。 如果工作区中不存在此环境，Azure 机器学习会创建它。 否则，Azure 机器学习会更新环境（如有必要）。 以下 JSON 是一个示例：

```json
{
    "entryScript": "score.py",
    "environment": {
        "docker": {
            "arguments": [],
            "baseDockerfile": null,
            "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
            "enabled": false,
            "sharedVolumes": true,
            "shmSize": null
        },
        "environmentVariables": {
            "EXAMPLE_ENV_VAR": "EXAMPLE_VALUE"
        },
        "name": "my-deploy-env",
        "python": {
            "baseCondaEnvironment": null,
            "condaDependencies": {
                "channels": [
                    "conda-forge"
                ],
                "dependencies": [
                    "python=3.6.2",
                    {
                        "pip": [
                            "azureml-defaults",
                            "azureml-telemetry",
                            "scikit-learn",
                            "inference-schema[numpy-support]"
                        ]
                    }
                ],
                "name": "project_environment"
            },
            "condaDependenciesFile": null,
            "interpreterPath": "python",
            "userManagedDependencies": false
        },
        "version": "1"
    }
}
```

还可以在单独的 CLI 参数中使用现有 Azure 机器学习[环境](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)，并从推理配置文件中删除“环境”密钥。 将 -e 用于环境名称，将 --ev 用于环境版本。 如果未指定 --ev，则会使用最新版本。 下面是推理配置文件的示例：

```json
{
    "entryScript": "score.py",
    "sourceDirectory": null
}
```

以下命令演示如何使用以前的推理配置文件（名为 myInferenceConfig.json）来部署模型。 

它还使用最新版本的现有 Azure 机器学习[环境](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)（名为 AzureML-Minimal）。

```azurecli
az ml model deploy -m mymodel:1 --ic myInferenceConfig.json -e AzureML-Minimal --dc deploymentconfig.json
```
