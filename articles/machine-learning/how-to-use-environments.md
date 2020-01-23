---
title: 创建可重用的 ML 环境
titleSuffix: Azure Machine Learning
description: 创建和管理用于模型训练和部署的环境。 管理环境的 Python 包和其他设置。
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 09/27/2019
ms.openlocfilehash: f052687a4dea998923bf439b5243f9999b8a293a
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75598070"
---
# <a name="reuse-environments-for-training--deployment-with-azure-machine-learning"></a>通过 Azure 机器学习重复使用用于训练和部署的环境。
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本文介绍如何创建和管理 Azure 机器学习[环境](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)以便在项目发展过程中跟踪和再现项目的软件依赖项。

软件依赖项管理是开发人员的常见任务。 你希望能在无需大量手动软件配置的情况下确保生成可以重现。 借助适用于本地开发的解决方案（如 pip 和 Conda），Azure 机器学习环境类为本地开发和分布式云开发提供了解决方案。

本文中的示例演示如何执行以下操作：

* 创建环境并指定包依赖项
* 检索和更新环境
* 使用环境进行训练
* 使用环境进行 Web 服务部署

## <a name="what-are-environments"></a>什么是环境

环境指定与训练和脚本评分有关的 Python 包、环境变量和软件设置以及运行时（Python、Spark 或 Docker）。 它们是 Azure 机器学习工作区中受到管理和版本控制的实体，可跨不同的计算目标支持可再现、可审核和可移植的机器学习工作流。

你可以在本地计算上使用环境对象来开发训练脚本、在 Azure 机器学习计算上重复使用同一环境进行大规模的模型训练，甚至可以使用相同的环境部署模型。

以下内容说明了在用于训练的运行配置中以及针对 Web 服务部署的推理和部署配置中，都可以使用相同的环境对象。

![图示：机器学习工作流中的环境](./media/how-to-use-environments/ml-environment.png)

### <a name="types-of-environments"></a>环境类型

环境可以总体分为三类：特选类、用户管理类以及系统管理类    。

特选环境由 Azure 机器学习提供，且默认可用于你的工作区。 它们包含 Python 包和设置的集合，有助于开启不同的机器学习框架。 

对于用户管理的环境，你需要负责设置环境，并在计算目标上安装训练脚本所需的每个包。 Conda 将不检查你的环境，也不会安装任何组件。 请注意，如果要定义自己的环境，必须将版本不低于 1.0.45 的 azureml-defaults 列为 Pip 依赖项。 此包包含将模型作为 Web 服务托管所需的功能。

若要通过 [Conda](https://conda.io/docs/) 管理 Python 环境和脚本依赖项，请使用系统管理的环境。 默认情况下，该服务将采用这种类型的环境，因为它适用于非手动配置的远程计算目标。

## <a name="prerequisites"></a>先决条件

* [安装](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)了适用于 Python 的 Azure 机器学习 SDK。
* [Azure 机器学习工作区](how-to-manage-workspace.md)。


## <a name="create-an-environment"></a>创建环境

可以采用多种方式创建试验环境。

### <a name="use-curated-environment"></a>使用特选环境

可以先选择一个特选环境。 

* __AzureML-Minimal__ 环境包含最小的一组包，用于启用运行跟踪和资产上传。 在创建自己的环境时，可以将其作为起点。

* __AzureML-Tutorial__ 环境包含常用数据科学包，如 Scikit-learn、Pandas 和 Matplotlib，以及更大的 azureml-sdk 包集。

特选环境由缓存的 Docker 映像支持，降低了运行准备成本。

使用 __Environment.get__ 方法选择一个特选环境：

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
env = Environment.get(workspace=ws, name="AzureML-Minimal")
```

可以使用以下代码列出特选环境以及它们的包：
```python
envs = Environment.list(workspace=ws)

for env in envs:
    if env.startswith("AzureML"):
        print("Name",env)
        print("packages", envs[env].python.conda_dependencies.serialize_to_string())
```

> [!WARNING]
>  在为自己的环境命名时，请勿使用 _AzureML_ 前缀。 此前缀专用于特选环境。

### <a name="instantiate-an-environment-object"></a>实例化环境对象

若要手动创建环境，请从 SDK 导入 Environment 类，然后使用以下代码实例化环境对象。

```python
from azureml.core.environment import Environment
Environment(name="myenv")
```

### <a name="conda-and-pip-specification-files"></a>Conda 和 pip 规范文件

还可以通过 Conda 规范或 pip 要求文件创建环境。
使用 [from_conda_specification()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-) 或 [from_pip_requirements()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-) 方法，并在方法参数中包含所需文件的环境名称和文件路径。

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

#From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv"
                                          file_path = "path-to-pip-requirements-file")
```

### <a name="existing-conda-environment"></a>现有 Conda 环境

如果本地计算机上存在现有 Conda 环境，则该服务将提供用于创建环境对象的解决方案。 这样就可以在远程运行上重用本地交互式环境。

以下代码使用 [from_existing_conda_environment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-) 方法从现有 Conda 环境 `mycondaenv` 中创建一个环境对象。

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

### <a name="automatically-create-environments"></a>自动创建环境

通过使用 submit() 方法提交训练运行来自动创建环境。 提交训练运行时，生成新环境可能需要几分钟时间，具体取决于所需依赖项的大小。 

如果在提交运行之前未在运行配置中指定环境，则会为你创建一个默认环境。

```python
from azureml.core import ScriptRunConfig, Experiment, Environment
# Create experiment 
myexp = Experiment(workspace=ws, name = "environment-example")

# Attaches training script and compute target to run config
runconfig = ScriptRunConfig(source_directory=".", script="example.py")
runconfig.run_config.target = "local"

# Submit the run
run = myexp.submit(config=runconfig)

# Shows each step of run 
run.wait_for_completion(show_output=True)
```

同样，如果使用 [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) 对象进行训练，可以直接将估算器实例提交为运行，而无需指定环境。 `Estimator` 对象已封装环境和计算目标。


## <a name="add-packages-to-an-environment"></a>将包添加到环境

使用 Conda、pip 或专用 wheel 文件将包添加到环境。 使用 [CondaDependency 类](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py)指定每个包依赖项，并将其添加到环境的 PythonSection 中。

### <a name="conda-and-pip-packages"></a>Conda 和 pip 包

如果某个包在 Conda 包存储库中可用，则相较于 pip 安装，我们更建议使用 Conda 安装。 因为 Conda 包通常附带预生成的二进制文件，能提高安装的可靠性。

以下示例分别使用 [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-) 方法和 [`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-) 方法将版本为 0.21.3 的 `scikit-learn` 以及 `pillow` 包添加到环境 `myenv`。

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs scikit-learn version 0.21.3 conda package
conda_dep.add_conda_package("scikit-learn==0.21.3")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

### <a name="private-wheel-files"></a>专用 wheel 文件

专用 pip wheel 文件的使用方法为：先使用静态 [`add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-) 方法将其上传到工作区存储，然后捕获存储 URL，再将 URL 传递到 `add_pip_package()` 方法。

```python
# During environment creation the service replaces the URL by secure SAS URL, so your wheel file is kept private and secure
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

## <a name="manage-environments"></a>管理环境

管理环境，可以跨计算目标以及其他工作区用户对环境进行更新、跟踪和重复使用。

### <a name="register-environments"></a>注册环境

在提交运行或部署 Web 服务时，环境将自动注册到工作区。 还可以使用 [register()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-) 方法手动注册环境。 通过注册环境，可让环境成为云中受到跟踪且经过版本控制的实体，还能在工作区用户之间共享环境。

以下代码将 `myenv` 环境注册到 `ws` 工作区。

```python
myenv.register(workspace=ws)
```

在首次使用环境时（用于训练或部署），环境将注册到工作区，并在计算目标上生成和部署。 环境由服务缓存。 与使用新服务或已更新的服务相比，重复使用已缓存的环境可以节约很多时间。

### <a name="get-existing-environments"></a>获取现有环境

Environment 类提供一些方法，通过这些方法可以按名称检索工作区中的现有环境，结果可以显示为列表，也可以按特定训练运行显示，用于进行故障排除或审核以及实现可再现性。

#### <a name="view-list-of-environments"></a>查看环境列表。

使用 [`Environment.list(workspace="workspace_name")`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-) 查看工作区中的环境，然后选择一个环境来重复使用。

#### <a name="get-environment-by-name"></a>按名称获取环境

还可以按名称和版本获取特定环境。
以下代码使用 [get()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-) 方法在 `ws` 工作区上检索 `1` 版本的 `myenv` 环境。

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="training-run-specific-environment"></a>特定于训练运行的环境

若要在训练完成后获取用于特定运行的环境，请使用 Run 类中的 [get_environment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--) 方法。

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>更新现有环境

如果对现有环境进行更改（如添加 Python 包），则在提交运行、部署模型或手动注册环境时，会创建一个环境的新版本。 该机制让你可以查看不同时间的环境更改。

若要更新现有环境的 Python 包版本，请指定该包的准确版本号。 若不指定准确的版本号，Azure 机器学习会重用创建环境时的包版本的现有环境。

### <a name="debug-the-image-build"></a>调试映像生成

此示例使用 [build()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace-) 方法将环境手动创建为 Docker 映像，并使用 [wait_for_completion()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-) 监视映像生成中的输出日志。 然后，生成的映像将显示在工作区 Azure 容器注册表下，这对调试很有帮助。

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

## <a name="docker-and-environments"></a>Docker 和环境

 使用 Azure 机器学习 `Environments` 类的 [DockerSection](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py)，可以详细地自定义和控制在其中执行训练运行的来宾操作系统。

在`enable` Docker 时，服务将生成一个 Docker 映像，并根据规范在该 Docker 容器中创建一个 Python 环境。 这为训练运行提供了额外的隔离性和可再现性。

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

默认情况下，生成的 Docker 映像会出现在与工作区关联的 Azure 容器注册表中。  存储库名称的格式为 azureml/azureml_\<uuid\>  唯一标识符 (uuid) 部分对应的是环境配置中计算出的哈希值  。 这让服务能确定是否已存在与给定环境对应的可重用映像。

此外，服务会自动使用一个基于 Ubuntu Linux 的[基础映像](https://github.com/Azure/AzureML-Containers)，并安装指定的 Python 包。 基础映像具有 CPU 和 GPU 版本。 Azure 机器学习会自动检测要使用的版本。

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
```

> [!NOTE]
> 如果在使用自定义 Docker 映像时指定 `environment.python.user_managed_dependencies=False`，服务将在该映像中生成 Conda 环境，并在该环境中执行运行，而不是使用可能已安装在基础映像上的 Python 库。 请将参数设置为 `True` 以使用自己安装的包。

## <a name="using-environments-for-training"></a>使用环境进行训练

若要提交训练运行，需要将你的环境、[计算目标](concept-compute-target.md) 和训练 Python 脚本合并到运行配置中，它是用于提交运行的包装对象。

提交训练运行时，生成新环境可能需要几分钟时间，具体取决于所需依赖项的大小。 环境由服务缓存，因此只要环境定义保持不变，完整的设置时间就只出现一次。

以下本地脚本运行示例显示会使用 [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) 作为包装对象的位置。

```python
from azureml.core import ScriptRunConfig, Experiment
from azureml.core.environment import Environment

exp = Experiment(name="myexp", workspace = ws)
# Instantiate environment
myenv = Environment(name="myenv")

# Add training script to run config
runconfig = ScriptRunConfig(source_directory=".", script="train.py")

# Attach compute target to run config
runconfig.run_config.target = "local"

# Attach environment to run config
runconfig.run_config.environment = myenv

# Submit run 
run = exp.submit(runconfig)
```

> [!NOTE]
> 若要禁用运行历史记录或运行快照，请使用 `ScriptRunConfig.run_config.history` 下的设置。

如果未在运行配置中指定环境，服务将在提交运行时为你创建一个默认环境。

### <a name="train-with-an-estimator"></a>使用估算器进行训练

如果使用[估算器](how-to-train-ml-models.md)进行训练，只需直接提交估算器实例，因为它已封装环境和计算目标。

以下代码将估算器用于 scikit-learn 模型的远程计算单节点训练运行，示例采用之前创建的计算目标 `compute_target` 以及数据存储对象 `ds`。

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])

# Submit the run 
run = experiment.submit(sk_est)
```

## <a name="using-environments-for-web-service-deployment"></a>使用环境进行 Web 服务部署

在将模型部署为 Web 服务时，可以使用环境。 使用环境能实现可重复的联网工作流，可在训练和推理计算中使用完全相同的库训练、测试和部署模型。

若要部署 web 服务，请在部署对象 ([deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)) 中合并环境、推理计算、评分脚本和已注册模型。 详细了解[部署 Web 服务](how-to-deploy-and-where.md)。

此示例假设你已完成训练运行，希望将模型部署到 Azure 容器实例 (ACI)。 生成 Web 服务时，模型和评分文件会装载到映像上，且会将 Azure 机器学习推理堆栈添加到映像。

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import AciWebservice, Webservice

# Register the model to deploy
model = run.register_model(model_name = "mymodel", model_path = "outputs/model.pkl")

# Combine scoring script & environment in Inference configuration
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

# Set deployment configuration
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)

# Define the model, inference & deployment configuration and web service name and location to deploy
service = Model.deploy(
    workspace = ws,
    name = "my_web_service",
    models = [model],
    inference_config = inference_config,
    deployment_config = deployment_config)
```

## <a name="example-notebooks"></a>示例笔记本

此[示例笔记本](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments)扩展了本文中演示的概念和方法。

## <a name="next-steps"></a>后续步骤

* [教程：训练模型](tutorial-train-models-with-aml.md)使用一个托管计算目标来训练模型。
* 训练模型后，了解[如何以及在何处部署模型](how-to-deploy-and-where.md)。
* 查看 [ 类](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) SDK 参考。
