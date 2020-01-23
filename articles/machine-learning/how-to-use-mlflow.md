---
title: 用于 ML 试验的 MLflow 跟踪
titleSuffix: Azure Machine Learning
description: 使用 Azure 机器学习设置 MLflow 以记录指标和项目，并部署来自 Databricks、本地环境或 VM 环境的模型。
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 09/23/2019
ms.custom: seodec18
ms.openlocfilehash: 04c7a553586faa380d0ca17f075590bc04fb14af
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75598041"
---
# <a name="track-metrics-and-deploy-models-with-mlflow-and-azure-machine-learning-preview"></a>使用 MLflow 和 Azure 机器学习跟踪指标并部署模型（预览）
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本文演示如何通过 Azure 机器学习来启用 MLflow 的跟踪 URI 和记录 API（两者统称为 [MLflow 跟踪](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api)）。 这样做可以实现以下目的：

+ 在 [Azure 机器学习工作区](/machine-learning/service/concept-azure-machine-learning-architecture#workspaces)中跟踪和记录试验指标及项目。 如果已为试验使用 MLflow 跟踪，工作区可提供集中、安全和可缩放的位置，用于存储训练指标和模型。

+ 将 MLflow 试验部署为 Azure 机器学习 Web 服务。 通过部署为 Web 服务，你可以将 Azure 机器学习监视和数据偏移检测功能应用到生产模型中。 

[MLflow](https://www.mlflow.org) 是一个开放源代码库，用于管理机器学习试验的生命周期。 MLFlow 跟踪是 MLflow 的一个组件，它可以记录和跟踪训练运行指标及模型项目，无论你的试验环境在远程计算机目标上、在虚拟机上、在本地计算机上，还是在 Azure Databricks 群集上。

下图说明使用 MLflow 跟踪，你可以跟踪试验的运行指标，并将模型项目存储在 Azure 机器学习工作区中。

![使用 Azure 机器学习的 MLflow 示意图](./media/how-to-use-mlflow/mlflow-diagram-track.png)

> [!TIP]
> 本文档中的信息主要是为希望监视模型训练过程的数据科学家和开发人员提供的。 如果您是一名管理员，希望监视 Azure 机器学习的资源使用情况和事件，例如配额、已完成的训练运行或已完成的模型部署，请参阅[监视 Azure 机器学习](monitor-azure-machine-learning.md)。

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>比较 MLflow 和 Azure 机器学习客户端

 下表汇总了可以使用 Azure 机器学习的不同客户端，以及它们各自的功能。

 MLflow 跟踪提供指标记录和项目存储功能，这些功能仅通过 [Azure 机器学习 Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) 提供。


| | MLflow 跟踪和部署 | Azure 机器学习 Python SDK |  Azure 机器学习 CLI | Azure 机器学习工作室|
|---|---|---|---|---|
| 管理工作区 |   | ✓ | ✓ | ✓ |
| 使用数据存储  |   | ✓ | ✓ | |
| 记录指标      | ✓ | ✓ |   | |
| 上传项目 | ✓ | ✓ |   | |
| 查看指标     | ✓ | ✓ | ✓ | ✓ |
| 管理计算   |   | ✓ | ✓ | ✓ |
| 部署模型    | ✓ | ✓ | ✓ | ✓ |
|监视模型性能||✓|  |   |
| 检测数据偏差 |   | ✓ |   | ✓ |

## <a name="prerequisites"></a>先决条件

* [安装 MLflow。](https://mlflow.org/docs/latest/quickstart.html)
* 在本地计算机上[安装 Azure 机器学习 SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)。该 SDK 为 MLflow 提供访问工作区的连接。
* [创建 Azure 机器学习工作区](how-to-manage-workspace.md)。

## <a name="track-local-runs"></a>跟踪本地运行

使用 Azure 机器学习进行 MLflow 跟踪，你可以将本地运行中记录的指标和项目存储到 Azure 机器学习工作区中。

安装 `azureml-contrib-run` 包，以通过 Azure 机器学习对在 Jupyter Notebook 或代码编辑器中本地运行的试验使用 MLflow 跟踪。

```shell
pip install azureml-mlflow
```

>[!NOTE]
>azureml.contrib 命名空间会频繁更改，因为我们正在改进服务。 因此，此命名空间中的任何内容都应被视为预览版，Microsoft 并不完全支持。

导入 `mlflow` 和 [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) 类以访问 MLflow 的跟踪 URI 并配置工作区。

在下面的代码中，`get_mlflow_tracking_uri()` 方法会向工作区 `ws` 分配唯一的跟踪 URI 地址，并且 `set_tracking_uri()` 会将 MLflow 跟踪 URI 指向该地址。

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>跟踪 URI 的有效时间不超过一小时。 如果在一段空闲时间后重新启动脚本，请使用 get_mlflow_tracking_uri API 来获取新的 URI。

使用 `set_experiment()` 设置 MLflow 试验名称，并通过 `start_run()` 启动训练运行。 然后使用 `log_metric()` 激活 MLflow 记录 API 并开始记录训练运行指标。

```Python
experiment_name = 'experiment_with_mlflow'
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>跟踪远程运行

使用 Azure 机器学习进行 MLflow 跟踪，你可以将远程运行中记录的指标和项目存储在 Azure 机器学习工作区中。

远程运行可以让你通过更强大的计算（例如启用 GPU 的虚拟机或机器学习计算群集）训练模型。 请参阅[为模型训练设置计算目标](how-to-set-up-training-targets.md)，了解不同的计算选项。

使用 [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) 类配置计算和训练运行环境。 将 `mlflow` 和 `azure-contrib-run` pip 包包含在环境的 [`CondaDependencies`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) 部分中。 然后，将远程计算作为计算目标构造 [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py)。

```Python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import ScriptRunConfig

exp = Experiment(workspace = 'my_workspace',
                 name='my_experiment')

mlflow_env = Environment(name='mlflow-env')

cd = CondaDependencies.create(pip_packages=['mlflow', 'azureml-contrib-run'])

mlflow_env.python.conda_dependencies = cd

src = ScriptRunConfig(source_directory='./my_script_location', script='my_training_script.py')

src.run_config.target = 'my-remote-compute-compute'
src.run_config.environment = mlflow_env
```

在训练脚本中，导入 `mlflow` 以使用 MLflow 记录 API，并开始记录运行指标。

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric('example', 1.23)
```

在具有此计算和训练运行配置的情况下，使用 `Experiment.submit('train.py')` 方法提交运行。 此方法会自动设置 MLflow 跟踪 URI 并将记录从 MLflow 定向到工作区。

```Python
run = exp.submit(src)
```

## <a name="track-azure-databricks-runs"></a>跟踪 Azure Databricks 运行

使用 Azure 机器学习进行 MLflow 跟踪，你可以将 Databricks 运行中记录的指标和项目存储在 Azure 机器学习工作区中。

要使用 Azure Databricks 运行 Mlflow 试验，你需要先创建一个 [Azure Databricks 工作区和群集](/azure-databricks/quickstart-create-databricks-workspace-portal)

在群集中，请务必从 PyPi 安装 azureml-mlflow 库，以确保群集有权访问所需的函数和类  。
然后，导入试验笔记本，将群集附加到试验笔记本，并运行试验。 

### <a name="install-libraries"></a>安装库

要在群集上安装库，请导航到“库”选项卡，然后单击“安装新库”  

 ![使用 Azure 机器学习的 MLflow 示意图](./media/how-to-use-mlflow/azure-databricks-cluster-libraries.png)

在“包”字段中，键入 azureml-mlflow，然后单击“安装”  。 根据需要重复此步骤，以便将其他额外的包安装到试验的群集。

 ![使用 Azure 机器学习的 MLflow 示意图](./media/how-to-use-mlflow/install-libraries.png)

### <a name="set-up-your-notebook-and-workspace"></a>设置笔记本和工作区

设置群集后，请导入试验笔记本，将其打开，并将群集附加到试验笔记本。

下面的代码应会出现在试验笔记本中。 此代码会获取你的 Azure 订阅详细信息，以实例化你的工作区。 此代码假定你拥有现成的资源组和 Azure 机器学习工作区。如果没有，你可以[创建它们](how-to-manage-workspace.md)。 

```python
import mlflow
import mlflow.azureml
import azureml.mlflow
import azureml.core

from azureml.core import Workspace
from azureml.mlflow import get_portal_url

subscription_id = 'subscription_id'

# Azure Machine Learning resource group NOT the managed resource group
resource_group = 'resource_group_name' 

#Azure Machine Learning workspace name, NOT Azure Databricks workspace
workspace_name = 'workspace_name'  

# Instantiate Azure Machine Learning workspace
ws = Workspace.get(name=workspace_name,
                   subscription_id=subscription_id,
                   resource_group=resource_group)
```

#### <a name="connect-your-azure-databricks-and-azure-machine-learning-workspaces"></a>连接 Azure Databricks 和 Azure 机器学习工作区

在 [Azure 门户](https://ms.portal.azure.cn)上，你可以将 Azure Databricks (ADB) 工作区链接到新的或现有的 Azure 机器学习工作区。 为此，请导航到 ADB 工作区，然后选择右下角的“链接 Azure 机器学习工作区”按钮  。 通过链接工作区，你可以在 Azure 机器学习工作区中跟踪试验数据。 

### <a name="link-mlflow-tracking-to-your-workspace"></a>将 MLflow 跟踪链接到工作区

实例化工作区后，设置 MLflow 跟踪 URI。 这样，便可以将 MLflow 跟踪链接到 Azure 机器学习工作区。 链接后，所有实验都将位于托管 Azure 机器学习跟踪服务中。

#### <a name="directly-set-mlflow-tracking-in-your-notebook"></a>直接在笔记本中设置 MLflow 跟踪

```python
uri = ws.get_mlflow_tracking_uri()
mlflow.set_tracking_uri(uri)
```

在训练脚本中，导入 mlflow 以使用 MLflow 记录 API，并开始记录运行指标。 下面的示例记录了时期丢失指标。 

```python
import mlflow 
mlflow.log_metric('epoch_loss', loss.item()) 
```

#### <a name="automate-setting-mlflow-tracking"></a>自动设置 MLflow 跟踪

与在群集上的每个后续试验笔记本会话中手动设置跟踪 URI 相比，你可以使用[Azure 机器学习跟踪群集初始化脚本](https://github.com/Azure/MachineLearningNotebooks/blob/3ce779063b000e0670bdd1acc6bc3a4ee707ec13/how-to-use-azureml/azure-databricks/linking/README.md)来自动执行此操作。

正确配置后，你可以通过 MLflow 用户界面或使用 MLflow 客户端，查看 Azure 机器学习 REST API 和所有客户端，以及 Azure Databricks 中的 MLflow 跟踪数据。

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>查看工作区中的指标和项目

MLflow 记录的指标和项目保存在工作区中。 要随时查看它们，请导航到工作区，然后在 [Azure 机器学习工作室](https://ml.azure.com)的工作区中按名称查找试验。  或者运行下面的代码。 

```python
run.get_metrics()
ws.get_details()
```

## <a name="deploy-mlflow-models-as-a-web-service"></a>将 MLflow 模型部署为 Web 服务

通过将 MLflow 试验作为 Azure 机器学习 Web 服务进行部署，你可以利用 Azure 机器学习模型管理和数据偏移检测功能，并将它们应用于生产模型。

下图演示了使用 MLflow 部署 API，你可以将现有 MLflow 模型部署为 Azure 机器学习 Web 服务，而无需考虑它们的框架（PyTorch、Tensorflow、scikit-learn、ONNX 等），并可在工作区中管理你的生产模型。

![使用 Azure 机器学习的 MLflow 示意图](./media/how-to-use-mlflow/mlflow-diagram-deploy.png)

### <a name="log-your-model"></a>记录模型

在可以部署之前，请确保已保存模型，以便可以引用它和它的部署路径位置。 在训练脚本中，应会有类似于以下 [mlflow.sklearn.log_model()](https://www.mlflow.org/docs/latest/python_api/mlflow.sklearn.html) 方法的代码，它会将模型保存到指定的输出目录。 

```python
# change sklearn to pytorch, tensorflow, etc. based on your experiment's framework 
import mlflow.sklearn

# Save the model to the outputs directory for capture
mlflow.sklearn.log_model(regression_model, model_save_path)
```
>[!NOTE]
> 请包含 `conda_env` 参数，以传递应在其中运行此模型的依赖项和环境的字典表示形式。

### <a name="retrieve-model-from-previous-run"></a>从以前的运行中检索模型

要检索运行，你需要运行 ID 和运行历史记录中保存模型的路径。 

```python
# gets the list of runs for your experiment as an array
experiment_name = 'experiment-with-mlflow'
exp = ws.experiments[experiment_name]
runs = list(exp.get_runs())

# get the run ID and the path in run history
runid = runs[0].id
model_save_path = 'model'
```

### <a name="create-docker-image"></a>创建 Docker 映像

`mlflow.azureml.build_image()` 函数以框架感知的方式从保存的模型生成 Docker 映像。 它会自动创建框架特定的推理包装器代码，并为你指定包依赖项。 指定模型路径、工作区、运行 ID 和其他参数。

下面的代码使用 runs:/<run.id>/model 作为 Scikit-learn 试验的 model_uri 路径，生成 docker 映像  。

```python
import mlflow.azureml

azure_image, azure_model = mlflow.azureml.build_image(model_uri='runs:/{}/{}'.format(runid, model_save_path),
                                                      workspace=ws,
                                                      model_name='sklearn-model',
                                                      image_name='sklearn-image',
                                                      synchronous=True)
```
创建 Docker 映像可能需要几分钟的时间。 

### <a name="deploy-the-docker-image"></a>部署 Docker 映像 

创建映像后，使用 Azure 机器学习 SDK 将映像部署为 Web 服务。

首先，请指定部署配置。 Azure 容器实例 (ACI) 适合用于快速的开发测试部署，而 Azure Kubernetes 服务 (AKS) 适合用于可缩放的生产部署。

#### <a name="deploy-to-aci"></a>部署到 ACI

使用 [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) 方法设置部署配置。 你还可以添加标记和说明来帮助跟踪你的 Web 服务。

```python
from azureml.core.webservice import AciWebservice, Webservice

# Configure 
aci_config = AciWebservice.deploy_configuration(cpu_cores=1, 
                                                memory_gb=1, 
                                                tags={'method' : 'sklearn'}, 
                                                description='Diabetes model',
                                                location='chinaeast')
```

然后，使用 Azure 机器学习 SDK [deploy_from_image()](https://docs.microsoft.com/azure/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none--overwrite-false-) 方法来部署映像。 

```python
webservice = Webservice.deploy_from_image( image=azure_image, 
                                           workspace=ws, 
                                           name='diabetes-model-1', 
                                           deployment_config=aci_config)

webservice.wait_for_deployment(show_output=True)
```
#### <a name="deploy-to-aks"></a>部署到 AKS

若要部署到 AKS，请先创建 AKS 群集，然后引入要部署的 Docker 映像。 在本示例中，我们从 ACI 部署中引入先前创建的映像。

要从之前的 ACI 部署中获取映像，请使用 [Image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image.image?view=azure-ml-py) 类。 

```python
from azureml.core.image import Image

# Get the image by name, you can change this based on the image you want to deploy
myimage = Image(workspace=ws, name='sklearn-image') 
```

使用 [ComputeTarget.create()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py#create-workspace--name--provisioning-configuration-) 方法创建 AKS 群集。 创建新群集可能需要 20-25 分钟。

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (can also provide parameters to customize)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aks-mlflow' 

# Create the cluster
aks_target = ComputeTarget.create(workspace=ws, 
                                  name=aks_name, 
                                  provisioning_configuration=prov_config)

aks_target.wait_for_completion(show_output = True)

print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```
使用 [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) 方法设置部署配置。 你还可以添加标记和说明来帮助跟踪你的 Web 服务。

```python
from azureml.core.webservice import Webservice, AksWebservice
from azureml.core.image import ContainerImage

# Set the web service configuration (using default here with app insights)
aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)

# Unique service name
service_name ='aks-service'
```

然后，使用 Azure 机器学习 SDK [deploy_from_image()](https://docs.microsoft.com/azure/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none--overwrite-false-) 方法来部署映像。 

```python
# Webservice creation using single command
aks_service = Webservice.deploy_from_image( workspace=ws, 
                                            name=service_name,
                                            deployment_config = aks_config
                                            image = myimage,
                                            deployment_target = aks_target)

aks_service.wait_for_deployment(show_output=True)
```

服务部署可能需要几分钟的时间。

## <a name="clean-up-resources"></a>清理资源

如果不打算使用工作区中记录的指标和项目，目前尚未提供单独删除它们的功能。 可以改为删除包含存储帐户和工作区的资源组，这样就不会产生任何费用：

1. 在 Azure 门户中，选择最左侧的“资源组”  。

   ![在 Azure 门户中删除](./media/how-to-use-mlflow/delete-resources.png)

1. 从列表中选择已创建的资源组。

1. 选择“删除资源组”  。

1. 输入资源组名称。 然后选择“删除”  。


## <a name="example-notebooks"></a>示例笔记本

[使用 Azure ML 笔记本的 MLflow](https://aka.ms/azureml-mlflow-examples) 演示并扩展了本文中介绍的概念。

## <a name="next-steps"></a>后续步骤
* [管理模型](concept-model-management-and-deployment.md)。
* 监视生产模型的[数据偏移](how-to-monitor-data-drift.md)情况。
