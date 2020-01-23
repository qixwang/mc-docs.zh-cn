---
title: 已知问题与故障排除
titleSuffix: Azure Machine Learning
description: 获取 Azure 机器学习的已知问题列表、解决方法和故障排除方法。
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d37dd448b2280bea0d6e3c0d1d3c326289540623
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75599582"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>Azure 机器学习的已知问题和故障排除

本文帮助你查找和更正使用 Azure 机器学习时遇到的错误或失败。

## <a name="outage-sr-iov-upgrade-to-ncv3-machines-in-amlcompute"></a>服务中断：将 SR-IOV 升级到 AmlCompute 中的 NCv3 计算机

从 2019 年 11 月初开始，Azure 计算会更新 NCv3 SKU 以支持所有 MPI 实现和版本，同时会更新配备 InfiniBand 的虚拟机的 RDMA 谓词。 这会造成短暂的停机 - [详细了解 SR-IOV 升级](https://azure.microsoft.com/updates/sriov-availability-on-ncv3-virtual-machines-sku)。

Azure 机器学习托管计算产品/服务 (AmlCompute) 的客户暂时不需要进行任何更改。 但是，需要根据[更新计划](https://azure.microsoft.com/updates/sr-iov-availability-schedule-on-ncv3-virtual-machines-sku)针对训练的短暂中断做好规划。 服务将负责更新群集节点上的 VM 映像，并自动扩展群集。 升级完成后，便可以使用其他所有 MPI 分发版（例如包含 Pytorch 的 OpenMPI），此外还可以获得更高的 InfiniBand 带宽、更低的延迟，以及更好的分布式应用程序性能。

## <a name="azure-machine-learning-designer-issues"></a>Azure 机器学习设计器问题

设计器的已知问题。

### <a name="long-compute-preparation-time"></a>计算准备时间很长

创建新的计算或唤醒正在退出的计算需要花费几分钟甚至更长的时间。 团队正在努力进行优化。


### <a name="cannot-run-an-experiment-only-contains-a-dataset"></a>无法运行仅包含数据集的试验 

你可能想要运行仅包含数据集的试验来可视化数据集。 但是，目前不允许运行仅包含数据集的试验。 我们正在积极解决此问题。
 
在问题得到解决之前，可将数据集连接到任何数据转换模块（“选择数据集中的列”，“编辑元数据”，“拆分数据”等），并运行试验。 然后，可将数据集可视化。 

下图演示了操作方法：![visulize-data](./media/resource-known-issues/aml-visualize-data.png)

## <a name="sdk-installation-issues"></a>SDK 安装问题

**错误消息：无法卸载 'PyYAML'**

适用于 Python 的 Azure 机器学习 SDK：PyYAML 是 distutils 安装的项目。 因此，在部分卸载的情况下，我们无法准确确定哪些文件属于它。 若要在忽略此错误的同时继续安装 SDK，请使用：

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

错误消息：`ERROR: No matching distribution found for azureml-dataprep-native` 

Anaconda Python 3.7.4 分发版中的一个 bug 会中断 azureml-sdk 的安装。 [此 GitHub 问题](https://github.com/ContinuumIO/anaconda-issues/issues/11195)中讨论了此问题。可以使用以下命令创建新 Conda 环境来解决此问题：
```bash
conda create -n <env-name> python=3.7.3
```
这会使用 Python 3.7.3 创建一个 Conda 环境，而该环境没有 3.7.4 中存在的安装问题。

## <a name="trouble-creating-azure-machine-learning-compute"></a>创建 Azure 机器学习计算时发生故障

如果用户在 GA 发布之前已通过 Azure 门户创建了自己的 Azure 机器学习工作区，则他们很可能无法在该工作区中创建 Azure 机器学习计算。 可对服务提出支持请求，也可通过门户或 SDK 创建新的工作区以立即解除锁定。

## <a name="image-building-failure"></a>映像生成失败

部署 Web 服务时映像生成失败。 解决方法是将“pynacl==1.2.1”作为 pip 依赖项添加到 Conda 文件以进行映像配置。

## <a name="deployment-failure"></a>部署失败

如果观察到 `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`，请将部署中使用的 VM 的 SKU 更改为具有更多内存的 SKU。

## <a name="fpgas"></a>FPGA

你将无法在 FPGA 上部署模型，直到已请求并获得 FPGA 配额批准为止。 若要请求访问权限，请填写配额请求表单： https://aka.ms/aml-real-time-ai

## <a name="automated-machine-learning"></a>自动化机器学习

Tensor Flow 自动化机器学习目前不支持 Tensor Flow 版本 1.13。 安装此版本会导致包依赖项停止工作。 我们正在努力解决此问题，以避免在将来的版本中出现。 

### <a name="experiment-charts"></a>试验图表

自 4 月 12 日以来，自动化 ML 试验迭代中显示的二元分类图表（精准率-召回率、ROC、增益曲线等）在用户界面中无法正常呈现。 绘制的图表目前显示相反的结果：表现更好的模型反而显示更低的结果。 我们研究解决方法。

## <a name="datasets-and-data-preparation"></a>数据集和数据准备

下面是 Azure 机器学习数据集的已知问题。

### <a name="typeerror-filenotfound-no-such-file-or-directory"></a>TypeError:FileNotFound:没有此类文件或目录

如果文件不在提供的文件路径中，则会出现此错误。 需确保引用文件的方式与在计算目标上将数据集装载到的位置相一致。 为确保确定性状态，我们建议在将数据集装载到计算目标时使用抽象路径。 例如，在以下代码中，我们将数据集装载到计算目标文件系统的根目录 `/tmp` 下。 

```python
# Note the leading / in '/tmp/dataset'
script_params = {
    '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
} 
```

如果不包含前导正斜杠“/”，则需要为计算目标上的工作目录添加前缀（例如 `/mnt/batch/.../tmp/dataset`），以指示要将数据集装载到的位置。 

### <a name="fail-to-read-parquet-file-from-http-or-adls-gen-2"></a>无法从 HTTP 或 ADLS Gen 2 读取 Parquet 文件

通过从 HTTP 或 ADLS Gen 2 读取 Parquet 文件创建数据集时，AzureML DataPrep SDK 版本 1.1.25 中的一个已知问题会导致失败。 此创建操作将会失败并显示 `Cannot seek once reading started.`。 若要解决此问题，请将 `azureml-dataprep` 升级到高于 1.1.26 的版本，或降级到低于 1.1.24 的版本。

```python
pip install --upgrade azureml-dataprep
```

### <a name="typeerror-mount-got-an-unexpected-keyword-argument-invocation_id"></a>TypeError: mount() 收到了意外的关键字参数“invocation_id”

如果 `azureml-core` 和 `azureml-dataprep` 的版本不兼容，则会出现此错误。 如果出现此错误，请将 `azureml-dataprep` 包升级到较高的版本（大于或等于 1.1.29）。

```python
pip install --upgrade azureml-dataprep
```

## <a name="databricks"></a>Databricks

Databricks 和 Azure 机器学习问题。

### <a name="failure-when-installing-packages"></a>安装包时失败

安装更多包时，Azure Databricks 上的 Azure 机器学习 SDK 安装失败。 某些包（如 `psutil`）可能会导致冲突。 为了避免安装错误，请通过冻结库版本来安装包。 此问题与 Databricks 相关，而与 Azure 机器学习 SDK 无关。 使用其他库时也可能会遇到此问题。 示例：

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

或者，如果一直面临 Python 库的安装问题，可以使用初始化脚本。 此方法并不正式受到支持。 有关详细信息，请参阅[群集范围的初始化脚本](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts)。

### <a name="cancel-an-automated-machine-learning-run"></a>取消自动化机器学习运行

在 Azure Databricks 上使用自动化机器学习功能时，若要取消某个运行并启动新的试验运行，请重启 Azure Databricks 群集。

### <a name="10-iterations-for-automated-machine-learning"></a>自动化机器学习的迭代数超过 10 个

在自动化机器学习设置中，如果迭代数超过 10 个，请在提交运行时将 `show_output` 设置为 `False`。

### <a name="widget-for-the-azure-machine-learning-sdk-and-automated-machine-learning"></a>Azure 机器学习 SDK 和自动化机器学习的小组件

Databricks 笔记本不支持 Azure 机器学习 SDK 小组件，因为笔记本无法分析 HTML 小组件。 可以通过在 Azure Databricks 笔记本单元中使用以下 Python 代码，在门户中查看该小组件：

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>导入错误:没有名为“pandas.core.indexes”的模块

如果在使用自动化机器学习时看到此错误：

1. 请运行以下命令，在 Azure Databricks 群集中安装两个包： 

   ```
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. 分离群集，然后将其重新附加到笔记本。 

如果这些步骤无法解决问题，请尝试重启群集。

### <a name="failtosendfeather"></a>FailToSendFeather

如果在 Azure Databricks 群集上读取数据时出现 `FailToSendFeather` 错误，请参考以下解决方法：

* 将 `azureml-sdk[automl]` 包升级到最新版本。
* 添加 `azureml-dataprep` 版本 1.1.8 或更高版本。
* 添加 `pyarrow` 版本 0.11 或更高版本。

## <a name="azure-portal"></a>Azure 门户

如果直接通过 SDK 或门户的共享链接查看工作区，则将无法在扩展程序中查看包含订阅信息的常规概述页。 也将无法切换到另一个工作区。 如果需要查看其他工作区，解决方法是直接转到 [Azure 机器学习工作室](https://ml.azure.com)并搜索工作区名称。

## <a name="diagnostic-logs"></a>诊断日志

如果在请求帮助时可以提供诊断信息，有时会很有帮助。 若要查看某些日志，请访问 [Azure 机器学习工作室](https://ml.azure.com)并转到你的工作区，然后选择“工作区”>“试验”>“运行”>“日志”。   

> [!NOTE]
> Azure 机器学习在训练期间记录会从各种源（例如，运行训练作业的 AutoML 或 Docker 容器）记录信息。 其中的许多日志没有详细的阐述。 如果遇到问题且联系了 Microsoft 支持部门，他们可以在排除故障时使用这些日志。

## <a name="activity-logs"></a>活动日志

Azure 机器学习工作区中的某些操作不会将信息记录到活动日志。  例如，启动训练运行或注册模型的操作。

其中的某些操作显示在工作区的“活动”区域中，但不会指出活动是谁发起的。 

## <a name="resource-quotas"></a>资源配额

了解使用 Azure 机器学习时可能遇到的[资源配额](how-to-manage-quotas.md)。

## <a name="authentication-errors"></a>身份验证错误

如果通过远程作业对某个计算目标执行管理操作，会收到以下错误之一：

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

例如，如果尝试通过一个为实施远程执行操作而提交的机器学习管道创建或附加计算目标，会收到错误。

## <a name="overloaded-azurefile-storage"></a>AzureFile 存储过载

如果收到 `Unable to upload project files to working directory in AzureFile because the storage is overloaded` 错误，请应用以下解决方法。

如果对其他工作负荷（例如数据传输）使用文件共享，则我们建议使用 Blob，以便可以自由使用文件共享来提交运行。 还可以在两个不同的工作区之间拆分工作负荷。

## <a name="webservices-in-azure-kubernetes-service-failures"></a>Azure Kubernetes 服务中的 Web 服务失败 

对于 Azure Kubernetes 服务中的许多 Web 服务失败，可以使用 `kubectl` 连接到群集进行调试。 可以运行以下命令获取 Azure Kubernetes 服务群集的 `kubeconfig.json`

```bash
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>更新 AKS 群集中的 Azure 机器学习组件

必须手动应用对 Azure Kubernetes 服务群集中安装的 Azure 机器学习组件的更新。 

> [!WARNING]
> 在执行以下操作之前，请检查 Azure Kubernetes 服务群集的版本。 如果群集版本等于或大于 1.14，则你无法将群集重新附加到 Azure 机器学习工作区。

可以通过从 Azure 机器学习工作区分离群集，然后将群集重新附加到工作区，来应用这些更新。 如果在群集中启用了 SSL，则重新附加群集时需要提供 SSL 证书和私钥。 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

如果不再具有 SSL 证书和私钥，或者使用的是 Azure 机器学习生成的证书，则可以在分离群集之前，使用 `kubectl` 连接到群集并检索机密 `azuremlfessl` 来检索文件。

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes 存储的机密采用 base-64 编码格式。 在将机密提供给 `attach_config.enable_ssl` 之前，需要对机密的 `cert.pem` 和 `key.pem` 组成部分进行 base-64 解码。 

## <a name="recommendations-for-error-fix"></a>错误修复的建议
下面根据我们的大致观察，提供了用于解决 Azure ML 中某些常见错误的建议。

### <a name="metric-document-is-too-large"></a>指标文档太大
对于一次性可从训练运行记录的指标对象大小，Azure 机器学习施加了内部限制。 如果在记录列表值指标时遇到“指标文档太大”错误，请尝试将列表拆分为较小的区块，例如：

```python
run.log_list("my metric name", my_metric[:N])
run.log_list("my metric name", my_metric[N:])
```

 在内部，运行历史记录服务会将具有相同指标名称的块串联到一个连续列表中。

### <a name="moduleerrors-no-module-named"></a>ModuleErrors（没有名为“xxx”的模块）
如果在 Azure ML 中提交试验时遇到 ModuleErrors，则表示训练脚本需要安装某个包，但并未添加该包。 提供包名称后，Azure ML 将在用于训练的环境中安装该包。 

如果使用[估算器](concept-azure-machine-learning-architecture.md#estimators)提交试验，则可以根据要从哪个源安装包，通过估算器中的 `pip_packages` 或 `conda_packages` 参数指定包名称。 还可以使用 `conda_dependencies_file` 指定包含所有依赖项的 yml 文件，或使用 `pip_requirements_file` 参数列出 txt 文件中的所有 pip 要求。

Azure ML 还提供适用于 Tensorflow、PyTorch、Chainer 和 SKLearn 的框架特定的估算器。 使用这些估算器可确保在用于训练的环境中自动安装框架依赖项。 可以使用相应的选项根据前面所述指定额外的依赖项。 
 
可以在 [AzureML 容器](https://github.com/Azure/AzureML-Containers)中看到 Azure ML 维护的 Docker 映像及其内容。
框架特定的依赖项列在相应的框架文档中 - [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks)、[PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks)、[TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks)、[SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks)。

> [!Note]
> 如果你认为某个特定的包比较常用，需要添加到 Azure ML 维护的映像和环境中，请在 [AzureML 容器](https://github.com/Azure/AzureML-Containers)中提出 GitHub 问题。 
 
 ### <a name="nameerror-name-not-defined-attributeerror-object-has-no-attribute"></a>NameError（未定义名称）、AttributeError（对象没有属性）
此异常应该是训练脚本引发的。 可以在 Azure 门户中查看日志文件，以获取有关未定义特定名称或属性错误的详细信息。 在 SDK 中，可以使用 `run.get_details()` 来查看错误消息。 这还会列出针对运行生成的所有日志文件。 请务必检查训练脚本，并在重试之前修复错误。 

### <a name="horovod-is-shut-down"></a>Horovod 已关闭
在大多数情况下，此异常表示某个进程中的根本性异常导致 horovod 关闭。 MPI 作业中的每个排名都会在 Azure ML 中生成专属的日志文件。 这些日志名为 `70_driver_logs`。 进行分布式训练时，日志名称带有 `_rank` 后缀，以方便区分日志。 若要查找导致 horovod 关闭的确切错误，请浏览所有日志文件，并查看 driver_log 文件末尾的 `Traceback`。 其中的某个文件会指出实际的根本性异常。 

## <a name="labeling-projects-issues"></a>标记项目问题

标记项目的已知问题。

### <a name="only-datasets-created-on-blob-datastores-can-be-used"></a>只能使用在 Blob 数据存储中创建的数据集

这是当前版本的已知限制。 

### <a name="after-creation-the-project-shows-initializing-for-a-long-time"></a>创建后，项目长时间显示“正在初始化”

手动刷新页面。 初始化应该按每秒大约 20 个数据点的速率继续。 缺少 autorefresh 是一个已知问题。 

### <a name="when-reviewing-images-newly-labeled-images-are-not-shown"></a>查看映像时，最近添加标签的映像不显示

若要加载所有带标签的映像，请选择“第一个”按钮。  按下“第一个”按钮会返回到列表的最前面，但会加载所有带标签的数据。 

### <a name="pressing-esc-key-while-labeling-for-object-detection-creates-a-zero-size-label-on-the-top-left-corner-submitting-labels-in-this-state-fails"></a>在为对象检测提供标记时按 Esc 键会在左上角创建大小为零的标签。 在此状态下提交标签会失败。

单击标签旁边的打叉标记来删除该标签。
