---
title: 访问 Azure 存储服务中的数据
titleSuffix: Azure Machine Learning
description: 了解如何在使用 Azure 机器学习训练期间使用数据存储安全连接到 Azure 存储服务
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: v-yiso
author: MayMSFT
ms.reviewer: nibaccam
origin.date: 01/15/2020
ms.date: 03/09/2020
ms.custom: seodec18
ms.openlocfilehash: 65e6c797e3f1bf4ff35050a98c105ce6e5a8a530
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80343563"
---
# <a name="access-data-in-azure-storage-services"></a>访问 Azure 存储服务中的数据
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本文介绍如何通过 Azure 机器学习数据存储轻松访问 Azure 存储服务中的数据。 数据存储存储连接信息，如订阅 ID 和令牌授权，因此你可以访问存储，无需在脚本中对其进行硬编码。 

可以由[这些 Azure 存储解决方案](#matrix)创建数据存储。 对于不支持的存储解决方案，为了在机器学习试验期间节省数据出口成本，建议[将数据移动](#move)到支持的 Azure 存储解决方案。 

## <a name="prerequisites"></a>先决条件
需要：
- Azure 订阅。 如果没有 Azure 订阅，请在开始之前创建一个免费帐户。 试用[免费版或付费版 Azure 机器学习](https://aka.ms/AMLFree)。

- 具有 [Azure blob 容器](/storage/blobs/storage-blobs-overview)或 [Azure 文件共享](/storage/files/storage-files-introduction)的 Azure 存储帐户。

- [适用于 Python 的 Azure 机器学习 SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)，或 [Azure 机器学习工作室](https://ml.azure.com/)的访问权限。

- Azure 机器学习工作区。
  
  [创建 Azure 机器学习工作区](how-to-manage-workspace.md)或通过 Python SDK 使用现有工作区。 导入 `Workspace` 和 `Datastore` 类，并使用函数 `from_config()` 从文件 `config.json` 中加载订阅信息。 默认情况下，这会查找当前目录中的 JSON 文件，但你也可以使用 `from_config(path="your/file/path")` 指定一个路径参数，使之指向该文件。

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```
<a name="matrix"></a>

## <a name="supported-data-storage-service-types"></a>支持的数据存储服务类型

数据存储目前支持将连接信息存储到以下矩阵中列出的存储服务。 目前不支持 Azure 数据仓库。 

| 存储类型 | 身份验证类型 | [Azure 机器学习工作室](https://ml.azure.com/) | [Azure 机器学习 Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) |  [Azure 机器学习 CLI](reference-azure-machine-learning-cli.md) | [Azure 机器学习 Rest API](https://docs.microsoft.com/rest/api/azureml/)
---|---|---|---|---|---
[Azure&nbsp;Blob&nbsp;存储](/storage/blobs/storage-blobs-overview)| 帐户密钥 <br> SAS 令牌 | ✓ | ✓ | ✓ |✓
[Azure 文件共享](/storage/files/storage-files-introduction)| 帐户密钥 <br> SAS 令牌 | ✓ | ✓ | ✓ |✓
[Azure&nbsp;Data Lake&nbsp;Storage Gen&nbsp;1](/data-lake-store/)| 服务主体| ✓ | ✓ | ✓ |✓
[Azure&nbsp;Data Lake&nbsp;Storage Gen&nbsp;2](/storage/blobs/data-lake-storage-introduction)| 服务主体| ✓ | ✓ | ✓ |✓
Azure&nbsp;SQL&nbsp;数据库| SQL 身份验证 <br>服务主体| ✓ | ✓ | ✓ |✓
Azure&nbsp;PostgreSQL | SQL 身份验证| ✓ | ✓ | ✓ |✓
Azure&nbsp;Database&nbsp;for&nbsp;MySQL | SQL 身份验证|  | ✓* | ✓* |✓*
Databricks&nbsp;文件系统| 无身份验证 | | ✓** | ✓ ** |✓** 

*仅管道 [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py) 支持 MySQL。 <br>
**仅管道 [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) 支持 Databricks

### <a name="storage-guidance"></a>存储指导原则

建议为 Azure Blob 容器创建数据存储。 标准和高级存储都可用于 blob。 尽管高级存储费用更高，但其吞吐速度也更快，可加速训练运行，特别是在针对大型数据集进行训练时。 有关存储帐户成本的信息，请参阅 [Azure 定价计算器](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service)。

创建工作区时，会将 Azure blob 容器和 Azure 文件共享自动注册到工作区。 它们分别名为 `workspaceblobstore` 和 `workspacefilestore`。 它们存储了在附加到工作区的存储帐户中预配的 blob 容器和文件共享的连接信息。 `workspaceblobstore` 容器设置为默认数据存储。

<a name="access"></a>

## <a name="create-and-register-datastores"></a>创建和注册数据存储

将 Azure 存储解决方案注册为数据存储时，会自动创建该数据存储并将其注册到特定工作区。 可以使用 Python SDK 或 Azure 机器学习工作室创建数据存储并将其注册到工作区。

>[!IMPORTANT]
> 在初始的数据存储创建和注册过程中，Azure 机器学习会验证基础存储服务是否存在，以及用户提供的主体（用户名、服务主体或 SAS 令牌）是否有权访问该存储。 但是，对于 Azure Data Lake Storage Gen1 和 Gen2 数据存储，此验证会在稍后调用 [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py) 或 [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-parquet-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) 之类的数据访问方法时进行。 
<br><br>
创建数据存储后，仅针对需要访问基础存储容器的方法执行此验证，而不是每次检索数据存储对象时都执行此验证  。 例如，如果要从数据存储中下载文件，则会进行验证，但如果只想更改默认数据存储，则不会进行验证。

### <a name="python-sdk"></a>Python SDK

所有注册方法都位于 [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) 类上，且具有 `register_azure_*` 格式。

可以在 [Azure 门户](https://portal.azure.com)中找到必要的信息来填充 `register()` 方法。
在左窗格中选择“存储帐户”  ，并选择要注册的存储帐户。 “概述”  页提供了帐户名称、容器和文件共享名称等信息。 

* 有关身份验证项目（如帐户密钥或 SAS 令牌），请在“设置”窗格中转到“帐户密钥”。 

* 对于租户 ID 和客户端 ID 等服务主体项，请转到  “应用注册”，然后选择要使用的应用。 其对应的“概览”  页会包含这些项。

> [!IMPORTANT]
> 如果存储帐户位于虚拟网络中，则仅支持通过 SDK  创建 Blob、文件共享、ADLS Gen 1 和 ADLS Gen 2 数据存储。 若要授权工作区访问存储帐户，请将参数 `grant_workspace_access` 设置为 `True`。

以下示例演示如何将 Azure blob 容器、Azure 文件共享和 Azure Data Lake Storage Gen2 注册为数据存储。 对于其他存储服务，请参阅[适用的 `register_azure_*` 方法的参考文档](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#methods)。

#### <a name="blob-container"></a>Blob 容器

若要将 Azure blob 容器注册为数据存储，请使用 [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-)。

以下代码会创建 `blob_datastore_name` 数据存储并将其注册到 `ws` 工作区。 此数据存储使用提供的帐户密钥访问 `my-account-name` 存储帐户中的 `my-container-name` blob 容器。

```Python
blob_datastore_name='azblobsdk' # Name of the datastore to workspace
container_name=os.getenv("BLOB_CONTAINER", "<my-container-name>") # Name of Azure blob container
account_name=os.getenv("BLOB_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("BLOB_ACCOUNT_KEY", "<my-account-key>") # Storage account key

blob_datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                         datastore_name=blob_datastore_name, 
                                                         container_name=container_name, 
                                                         account_name=account_name,
                                                         account_key=account_key)
```

#### <a name="file-share"></a>文件共享

若要将 Azure 文件共享注册为数据存储，请使用 [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-)。 

以下代码会创建 `file_datastore_name` 数据存储并将其注册到 `ws` 工作区。 此数据存储使用提供的帐户密钥访问 `my-account-name` 存储帐户中的 `my-fileshare-name` 文件共享。

```Python
file_datastore_name='azfilesharesdk' # Name of the datastore to workspace
file_share_name=os.getenv("FILE_SHARE_CONTAINER", "<my-fileshare-name>") # Name of Azure file share container
account_name=os.getenv("FILE_SHARE_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("FILE_SHARE_ACCOUNT_KEY", "<my-account-key>") # Storage account key

file_datastore = Datastore.register_azure_file_share(workspace=ws,
                                                 datastore_name=file_datastore_name, 
                                                 file_share_name=file_share_name, 
                                                 account_name=account_name,
                                                 account_key=account_key)
```

#### <a name="azure-data-lake-storage-generation-2"></a>Azure Data Lake Storage Gen2

对于 Azure Data Lake Storage Gen2 (ADLS Gen2)数据存储，请使用 [register_azure_data_lake_gen2()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-data-lake-gen2-workspace--datastore-name--filesystem--account-name--tenant-id--client-id--client-secret--resource-url-none--authority-url-none--protocol-none--endpoint-none--overwrite-false-) 注册通过[服务主体权限](/active-directory/develop/howto-create-service-principal-portal)连接到 Azure DataLake Gen2 存储的凭据数据存储。 若要利用服务主体，则需[注册应用程序](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)并将角色分配设置为“读者”和“数据访问”。 详细了解 [ADLS Gen2 的访问控制设置](/storage/blobs/data-lake-storage-access-control)。 

以下代码会创建 `adlsgen2_datastore_name` 数据存储并将其注册到 `ws` 工作区。 此数据存储使用提供的服务主体凭据访问 `account_name` 存储帐户中的 `test` 文件系统。

```python 
adlsgen2_datastore_name = 'adlsgen2datastore'

subscription_id=os.getenv("ADL_SUBSCRIPTION", "<my_subscription_id>") # subscription id of ADLS account
resource_group=os.getenv("ADL_RESOURCE_GROUP", "<my_resource_group>") # resource group of ADLS account

account_name=os.getenv("ADLSGEN2_ACCOUNTNAME", "<my_account_name>") # ADLS Gen2 account name
tenant_id=os.getenv("ADLSGEN2_TENANT", "<my_tenant_id>") # tenant id of service principal
client_id=os.getenv("ADLSGEN2_CLIENTID", "<my_client_id>") # client id of service principal
client_secret=os.getenv("ADLSGEN2_CLIENT_SECRET", "<my_client_secret>") # the secret of service principal

adlsgen2_datastore = Datastore.register_azure_data_lake_gen2(workspace=ws,
                                                             datastore_name=adlsgen2_datastore_name,
                                                             account_name=account_name, # ADLS Gen2 account name
                                                             filesystem='test', # ADLS Gen2 filesystem
                                                             tenant_id=tenant_id, # tenant id of service principal
                                                             client_id=client_id, # client id of service principal
                                                             client_secret=client_secret) # the secret of service principal
```

### <a name="azure-machine-learning-studio"></a>Azure 机器学习工作室 

在 Azure 机器学习工作室中通过几个步骤创建新的数据存储：

> [!IMPORTANT]
> 如果存储帐户位于虚拟网络中，则仅支持[通过 SDK](#python-sdk) 创建数据存储。 

1. 登录到 [Azure 机器学习工作室](https://ml.azure.com/)。
1. 在左窗格中的“管理”下，选择“数据存储”   。
1. 选择“+ 新建数据存储”  。
1. 填写表单以创建新的数据存储。 表单将根据你选择的 Azure 存储类型和身份验证类型智能更新。
  
可以使用 [Azure 门户](https://portal.azure.cn)找到必要的信息来填充该表单。 在左窗格中选择“存储帐户”  ，并选择要注册的存储帐户。 “概述”  页提供了帐户名称、容器和文件共享名称等信息。 

* 有关身份验证项目（如帐户密钥或 SAS 令牌），请在“设置”窗格中转到“帐户密钥”。 

* 对于租户 ID 和客户端 ID 等服务主体项，请转到  “应用注册”，然后选择要使用的应用。 其对应的“概览”  页会包含这些项。 

下面的示例演示创建 Azure blob 数据存储时表单的外观： 
    
![新数据存储的表单](media/how-to-access-data/new-datastore-form.png)


<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>从工作区获取数据存储

> [!IMPORTANT]
> Azure 机器学习设计器（预览版）会在你打开设计器主页中的示例时自动创建一个名为 **azureml_globaldatasets** 的数据存储。 此数据存储仅包含示例数据集。 请勿  将此数据存储用于任何机密数据访问！
> ![设计器示例数据集的自动创建的数据存储](media/how-to-access-data/datastore-designer-sample.png)

若要获取在当前工作区中注册的特定数据存储，请在 `Datastore` 类上使用 [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) 静态方法：

```Python
# Get a named datastore from the current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
若要获取在给定工作区中注册的数据存储的列表，可以在工作区对象上使用 [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores) 属性：

```Python
# List all datastores registered in the current workspace
datastores = ws.datastores
for name, datastore in datastores.items():
    print(name, datastore.datastore_type)
```

若要获取工作区的默认数据存储，请使用以下行：

```Python
datastore = ws.get_default_datastore()
```

若要为当前工作区定义不同的默认数据存储，请在工作区对象上使用 [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-) 方法：

```Python
# Define the default datastore for the current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload-and-download-data"></a>上传和下载数据

下面的示例中所述的 [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#upload-src-dir--target-path-none--overwrite-false--show-progress-true-) 和 [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) 方法特定于 [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) 和 [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) 类，且对于这两个类的操作相同。

### <a name="upload"></a>上传

使用 Python SDK 将目录或独立文件上传到数据存储：

```Python
datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

`target_path` 参数指定要上传的文件共享（或 blob 容器）中的位置。 它默认为 `None`，因此数据上传到根目录。 如果 `overwrite=True`，则将覆盖 `target_path` 处的任何现有数据。

也可通过 `upload_files()` 方法将一系列独立文件上传到数据存储。

### <a name="download"></a>下载

将数据从数据存储下载到本地文件系统：

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

`target_path` 参数是要将数据下载到的本地目录位置。 若要在文件共享（或 blob 容器）中指定要下载到的文件夹路径，请提供 `prefix` 的路径。 如果 `prefix` 为 `None`，将下载文件共享（或 blob 容器）的所有内容。

<a name="train"></a>
## <a name="access-your-data-during-training"></a>在训练期间访问数据

若要与数据存储中的数据交互，或将数据打包成机器学习任务（例如训练）的可用对象，请[创建 Azure 机器学习数据集](how-to-create-register-datasets.md)。 数据集提供将表格数据加载到 Pandas 或 Spark 数据帧的功能。 数据集还提供从 Azure Blob 存储、Azure 文件存储、Azure Data Lake Storage Gen1、Azure Data Lake Storage Gen2、Azure SQL 数据库和 Azure Database for PostgreSQL 下载或装载任何格式的文件的功能。 [详细了解如何使用数据集进行训练](how-to-train-with-datasets.md)。

### <a name="accessing-source-code-during-training"></a>在训练期间访问源代码

Azure Blob 存储具有比 Azure 文件共享更快的吞吐速度，并将扩展到大量并行启动的作业。 出于此原因，建议将运行配置为使用 Blob 存储来传输源代码文件。

以下代码示例在运行配置中指定用于源代码传输的 blob 数据存储：

```python 
# workspaceblobstore is the default blob storage
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="access-data-during-scoring"></a>在评分期间访问数据

Azure 机器学习提供多种方法来使用模型进行评分。 其中一些方法不提供对数据存储的访问权限。 使用下表了解允许在评分期间访问数据存储的方法：

| 方法 | 数据存储访问 | 说明 |
| ----- | :-----: | ----- |
| [批量预测](how-to-use-parallel-run-step.md) | ✔ | 以异步方式对大量数据进行预测。 |
| [Web 服务](how-to-deploy-and-where.md) | &nbsp; | 将模型部署为 Web 服务。 |
| [Azure IoT Edge 模块](how-to-deploy-and-where.md) | &nbsp; | 将模型部署到 IoT Edge 设备。 |

对于 SDK 不提供对数据存储的访问权限的情况，也许可以通过使用相关 Azure SDK 访问数据以创建自定义代码。 例如，可使用[适用于 Python 的 Azure 存储 SDK](https://github.com/Azure/azure-storage-python) 这一客户端库来访问存储在 blob 或文件中的数据。

<a name="move"></a>
## <a name="move-data-to-supported-azure-storage-solutions"></a>将数据移动到支持的 Azure 存储解决方案

Azure 机器学习支持访问 Azure Blob 存储、Azure 文件存储、Azure Data Lake Storage Gen2、Azure SQL 数据库和 Azure Database for PostgreSQL 中的数据。 如果使用不支持的存储，建议使用 [Azure 数据工厂和这些步骤](/data-factory/quickstart-create-data-factory-copy-data-tool)将数据移到支持的 Azure 存储解决方案。 将数据移动到支持的存储有助于在机器学习试验期间节省数据出口成本。 

Azure 数据工厂具有超过 80 个预生成的连接器，可提供高效且可复原的数据传输，无需额外付费。 这些连接器包括 Azure 数据服务、本地数据源、Amazon S3 和 Redshift 以及 Google BigQuery。

## <a name="next-steps"></a>后续步骤

* [创建 Azure 机器学习数据集](how-to-create-register-datasets.md)
* [定型模型](how-to-train-ml-models.md)
* [部署模型](how-to-deploy-and-where.md)
