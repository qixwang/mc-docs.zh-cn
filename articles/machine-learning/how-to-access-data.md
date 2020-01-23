---
title: 访问 Azure 存储服务中的数据
titleSuffix: Azure Machine Learning
description: 了解如何在使用 Azure 机器学习训练期间使用数据存储安全连接到 Azure 存储服务
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: ylxiong
author: YLXiong1125
ms.reviewer: nibaccam
ms.date: 12/10/2019
ms.custom: seodec18
ms.openlocfilehash: b1324f4c27249344738254f99d7a4c6824b225b5
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75598865"
---
# <a name="access-data-in-azure-storage-services"></a>访问 Azure 存储服务中的数据
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本文介绍如何通过 Azure 机器学习数据存储轻松访问 Azure 存储服务中的数据。 数据存储用于存储连接信息，如订阅 ID 和令牌授权。 使用数据存储时，无需在脚本中对连接信息进行硬编码即可访问存储。 

可以由[这些 Azure 存储解决方案](#matrix)创建数据存储。 对于不支持的存储解决方案，为了在机器学习试验期间节省数据出口成本，建议[将数据移动](#move)到支持的 Azure 存储解决方案。 

## <a name="prerequisites"></a>先决条件
需要：
- Azure 订阅。 如果没有 Azure 订阅，请在开始之前创建一个免费帐户。 试用[免费版或付费版 Azure 机器学习](https://aka.ms/AMLFree)。

- 具有 [Azure blob 容器](/storage/blobs/storage-blobs-overview)或 [Azure 文件共享](/storage/files/storage-files-introduction)的 Azure 存储帐户。

- [适用于 Python 的 Azure 机器学习 SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)，或 [Azure 机器学习工作室](https://ml.azure.com/)的访问权限。

- Azure 机器学习工作区。
  
  [创建 Azure 机器学习工作区](how-to-manage-workspace.md)或通过 Python SDK 使用现有工作区：

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

<a name="access"></a>

## <a name="create-and-register-datastores"></a>创建和注册数据存储

将 Azure 存储解决方案注册为数据存储时，会在特定工作区中自动创建该数据存储。 可以使用 Python SDK 或 Azure 机器学习工作室创建数据存储并将其注册到工作区。

### <a name="python-sdk"></a>Python SDK

所有注册方法都位于 [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) 类上，且具有 `register_azure_*` 格式。

可以使用 [Azure 门户](https://portal.azure.cn)找到必要的信息来填充 `register()` 方法：

1. 在左窗格中选择“存储帐户”  ，并选择要注册的存储帐户。 
2. 有关帐户名称、容器和文件共享名称等信息，请转到“概述”  页。 有关身份验证信息（如帐户密钥或 SAS 令牌），请在“设置”  窗格中转到“访问密钥”  。 

> [!IMPORTANT]
> 如果存储帐户位于虚拟网络中，则仅支持创建 Azure blob 数据存储。 若要授权工作区访问存储帐户，请将参数 `grant_workspace_access` 设置为 `True`。

以下示例演示如何将 Azure blob 容器、Azure 文件共享和 Azure SQL 数据注册为数据存储。

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

#### <a name="sql-data"></a>SQL 数据

对于 Azure SQL 数据存储，请使用 [register_azure_sql_database()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-sql-database-workspace--datastore-name--server-name--database-name--tenant-id-none--client-id-none--client-secret-none--resource-url-none--authority-url-none--endpoint-none--overwrite-false--username-none--password-none-) 来注册使用 SQL 身份验证或服务主体权限连接到 Azure SQL 数据库的凭据数据存储。 

通过 SQL 身份验证进行注册：

```python
sql_datastore_name="azsqlsdksql"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the Azure SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the Azure SQL database
username=os.getenv("SQL_USER_NAME", "<my-sql-user-name>") # Username of the database user to access the database
password=os.getenv("SQL_USER_PASSWORD", "<my-sql-user-password>") # Password of the database user to access the database

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                  datastore_name=sql_datastore_name,
                                                  server_name=server_name,
                                                  database_name=database_name,
                                                  username=username,
                                                  password=password)

```

通过服务主体进行注册：

```python 
sql_datastore_name="azsqlsdksp"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the SQL database
client_id=os.getenv("SQL_CLIENTNAME", "<my-client-id>") # Client ID of the service principal with permissions to access the database
client_secret=os.getenv("SQL_CLIENTSECRET", "<my-client-secret>") # Secret of the service principal
tenant_id=os.getenv("SQL_TENANTID", "<my-tenant-id>") # Tenant ID of the service principal

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                      datastore_name=sql_datastore_name,
                                                      server_name=server_name,
                                                      database_name=database_name,
                                                      client_id=client_id,
                                                      client_secret=client_secret,
                                                      tenant_id=tenant_id)
```

#### <a name="storage-guidance"></a>存储指导原则

建议使用 Azure blob 容器。 标准和高级存储都可用于 blob。 尽管高级存储费用更高，但其吞吐速度也更快，可加速训练运行，特别是在针对大型数据集进行训练时。 有关存储帐户成本的信息，请参阅 [Azure 定价计算器](https://www.azure.cn/pricing/calculator/?service=machine-learning-service)。

### <a name="azure-machine-learning-studio"></a>Azure 机器学习工作室 

在 Azure 机器学习工作室中通过几个步骤创建新的数据存储：

1. 登录到 [Azure 机器学习工作室](https://ml.azure.com/)。
1. 在左窗格中的“管理”下，选择“数据存储”   。
1. 选择“+ 新建数据存储”  。
1. 填写表单以创建新的数据存储。 表单将根据你选择的 Azure 存储类型和身份验证类型智能更新。
  
可以使用 [Azure 门户](https://portal.azure.cn)找到必要的信息来填充该表单。 在左窗格中选择“存储帐户”  ，并选择要注册的存储帐户。 “概述”  页提供了帐户名称、容器和文件共享名称等信息。 有关身份验证项目（如帐户密钥或 SAS 令牌），请在“设置”  窗格中转到“帐户密钥”  。

下面的示例演示创建 Azure blob 数据存储时表单的外观： 
    
![新数据存储的表单](media/how-to-access-data/new-datastore-form.png)


<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>从工作区获取数据存储

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

创建工作区时，会将 Azure blob 容器和 Azure 文件共享自动注册到工作区。 它们分别名为 `workspaceblobstore` 和 `workspacefilestore`。 它们存储了在附加到工作区的存储帐户中预配的 blob 容器和文件共享的连接信息。 `workspaceblobstore` 容器设置为默认数据存储。

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

> [!IMPORTANT]
> 现建议使用 [Azure 机器学习数据集](how-to-create-register-datasets.md)在训练期间访问数据。 数据集提供将表格数据加载到 Pandas 或 Spark 数据帧的功能。 数据集还提供从 Azure Blob 存储、Azure 文件存储、Azure Data Lake Storage Gen1、Azure Data Lake Storage Gen2、Azure SQL 数据库和 Azure Database for PostgreSQL 下载或装载任何格式的文件的功能。 [详细了解如何使用数据集进行训练](how-to-train-with-datasets.md)。

下表列出了一些方法，这些方法会在运行期间告诉计算目标如何使用数据存储： 

方式|方法|说明|
----|-----|--------
装载| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-mount--)| 用于在计算目标上装载数据存储。 装载数据存储后，计算目标即可访问数据存储中的所有文件。
下载|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-download-path-on-compute-none-)|用于将数据存储的内容下载到 `path_on_compute` 指定的位置。 <br><br> 此下载发生在运行之前。
上传|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-upload-path-on-compute-none-)| 用于将文件从 `path_on_compute` 指定的位置上传到数据存储。 <br><br> 此上传发生在运行之后。

若要引用数据存储中的特定文件夹或文件并使其在计算目标上可用，请使用数据存储 [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#path-path-none--data-reference-name-none-) 方法：

```Python
# To mount the full contents in your storage to the compute target
datastore.as_mount()

# To download the contents of only the `./bar` directory in your storage to the compute target
datastore.path('./bar').as_download()
```
> [!NOTE]
> 任何指定的 `datastore` 或 `datastore.path` 对象都解析为 `"$AZUREML_DATAREFERENCE_XXXX"` 格式的环境变量名称。 此名称的值表示计算目标上的装载/下载路径。 计算目标上的数据存储路径可能与训练脚本的执行路径不同。

### <a name="examples"></a>示例 

建议在训练期间使用 [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) 类访问数据。 

`script_params` 变量是包含 `entry_script` 的参数的字典。 使用它传入数据存储，并描述如何在计算目标上提供数据。 有关详细信息，请参阅[端到端教程](tutorial-train-models-with-aml.md)。

```Python
from azureml.train.estimator import Estimator

# Notice that '/' is in front, which indicates the absolute path
script_params = {
    '--data_dir': datastore.path('/bar').as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

也可将数据存储列表传递到 `Estimator` 构造函数的 `inputs` 参数，以便将数据装载或复制到数据存储，或从数据存储进行装载或复制。 此代码示例：
* 在运行 `train.py` 训练脚本之前，将 `datastore1` 中的所有内容下载到计算目标。
* 在运行 `train.py` 之前，将 `datastore2` 中的 `'./foo'` 文件夹下载到计算目标。
* 运行脚本后，将 `'./bar.pkl'` 文件从计算目标上传到 `datastore3`。

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[datastore1.as_download(), datastore2.path('./foo').as_download(), datastore3.as_upload(path_on_compute='./bar.pkl')])
```
如果更愿意使用 `RunConfig` 对象进行训练，则需要设置 [`DataReference`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) 对象。 

以下代码演示如何在估算管道中使用 `DataReference` 对象。 有关完整示例，请参阅[此笔记本](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-estimatorstep.ipynb)。

```Python
from azureml.core import Datastore
from azureml.data.data_reference import DataReference
from azureml.pipeline.core import PipelineData

def_blob_store = Datastore(ws, "workspaceblobstore")

input_data = DataReference(
       datastore=def_blob_store,
       data_reference_name="input_data",
       path_on_datastore="20newsgroups/20news.pkl")

output = PipelineData("output", datastore=def_blob_store)
```
<a name="matrix"></a>

### <a name="compute-and-datastore-matrix"></a>计算和数据存储矩阵

数据存储目前支持将连接信息存储到以下矩阵中列出的存储服务。 此矩阵显示了不同计算目标和数据存储方案的可用数据访问功能。 [详细了解 Azure 机器学习的计算目标](how-to-set-up-training-targets.md#compute-targets-for-training)。

|计算|[AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)                                       |[AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)                                      |[AzureDataLakeDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakedatastore?view=azure-ml-py) |[AzureDataLakeGen2Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) [AzurePostgreSqlDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_postgre_sql_datastore.azurepostgresqldatastore?view=azure-ml-py) [AzureSqlDatabaseDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_sql_database_datastore.azuresqldatabasedatastore?view=azure-ml-py) |
|--------------------------------|----------------------------------------------------------|----------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------|
| Local|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|不适用         |不适用                                                                         |
| Azure 机器学习计算 |[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)、[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)[as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)[机器学习管道](concept-ml-pipelines.md)|[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)、[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)[as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)[机器学习管道](concept-ml-pipelines.md)|不适用         |不适用                                                                         |
| 虚拟机               |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                           | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |不适用         |不适用                                                                         |
| Azure HDInsight                      |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |不适用         |不适用                                                                         |
| 数据传输                  |[机器学习管道](concept-ml-pipelines.md)                                               |不适用                                           |[机器学习管道](concept-ml-pipelines.md)            |[机器学习管道](concept-ml-pipelines.md)                                                                            |
| Azure Databricks                     |[机器学习管道](concept-ml-pipelines.md)                                              |不适用                                           |[机器学习管道](concept-ml-pipelines.md)             |不适用                                                                         |
| Azure Batch                    |[机器学习管道](concept-ml-pipelines.md)                                               |不适用                                           |不适用         |不适用                                                                         |
| Azure Data Lake Analytics       |不适用                                           |不适用                                           |[机器学习管道](concept-ml-pipelines.md)             |不适用                                                                         |

> [!NOTE]
> 在某些情况下，使用 `as_download()` 而不是 `as_mount()` 时，迭代性强的大型数据进程可以更快地运行。 可以通过试验来验证这一点。

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
| [批量预测](how-to-run-batch-predictions.md) | ✔ | 以异步方式对大量数据进行预测。 |
| [Web 服务](how-to-deploy-and-where.md) | &nbsp; | 将模型部署为 Web 服务。 |
| [Azure IoT Edge 模块](how-to-deploy-and-where.md) | &nbsp; | 将模型部署到 IoT Edge 设备。 |

对于 SDK 不提供对数据存储的访问权限的情况，也许可以通过使用相关 Azure SDK 访问数据以创建自定义代码。 例如，可使用[适用于 Python 的 Azure 存储 SDK](https://github.com/Azure/azure-storage-python) 这一客户端库来访问存储在 blob 或文件中的数据。

<a name="move"></a>
## <a name="move-data-to-supported-azure-storage-solutions"></a>将数据移动到支持的 Azure 存储解决方案

Azure 机器学习支持访问 Azure Blob 存储、Azure 文件存储、Azure Data Lake Storage Gen1、Azure Data Lake Storage Gen2、Azure SQL 数据库和 Azure Database for PostgreSQL 中的数据。 如果使用不支持的存储，建议使用 Azure 数据工厂将数据移动到支持的 Azure 存储解决方案。 将数据移动到支持的存储有助于在机器学习试验期间节省数据出口成本。 

Azure 数据工厂具有超过 80 个预生成的连接器，可提供高效且可复原的数据传输，无需额外付费。 这些连接器包括 Azure 数据服务、本地数据源、Amazon S3 和 Redshift 以及 Google BigQuery。 [按照分步指南，使用 Azure 数据工厂移动数据](/data-factory/quickstart-create-data-factory-copy-data-tool)。

## <a name="next-steps"></a>后续步骤

* [定型模型](how-to-train-ml-models.md)
* [部署模型](how-to-deploy-and-where.md)
