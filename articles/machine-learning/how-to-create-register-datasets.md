---
title: 创建 Azure 机器学习数据集以访问数据
titleSuffix: Azure Machine Learning
description: 了解如何创建 Azure 机器学习数据集以访问机器学习试验运行的数据。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 11/04/2019
ms.openlocfilehash: 16da4930143c8e3fee0642d0e18dd98494d641b9
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75599620"
---
# <a name="create-azure-machine-learning-datasets"></a>创建 Azure 机器学习数据集

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本文介绍如何创建 Azure 机器学习数据集，以访问本地或远程试验的数据。

使用 Azure 机器学习数据集可以：

* 在存储中保留数据的单个副本，供数据集引用。

* 在模型训练期间无缝访问数据，而无需考虑连接字符串或数据路径。

* 与其他用户共享数据和展开协作。

## <a name="prerequisites"></a>先决条件

若要创建和使用数据集，需要做好以下准备：

* Azure 订阅。 如果没有订阅，请在开始之前创建一个免费帐户。 试用[免费版或付费版 Azure 机器学习](https://aka.ms/AMLFree)。

* 一个 [Azure 机器学习工作区](how-to-manage-workspace.md)。

* [已安装适用于 Python 的 Azure 机器学习 SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)，其中包含 azureml-datasets 包。

> [!NOTE]
> 某些数据集类依赖于 [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) 包。 对于 Linux 用户，只有以下分发版支持这些类：Red Hat Enterprise Linux、Ubuntu、Fedora 和 CentOS。

## <a name="dataset-types"></a>数据集类型

根据用户在训练中使用数据集的方式，存在两种数据集类型：

* [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) 表示表格格式的、通过分析提供的文件或文件列表所创建的数据。 使用此类数据集可将数据具体化为 Pandas 或 Spark 数据帧。 可以从 .csv、.tsv 和 parquet 文件以及从 SQL 查询结果创建 `TabularDataset` 对象。 有关完整列表，请参阅 [TabularDatasetFactory 类](https://aka.ms/tabulardataset-api-reference)。

* [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) 类引用数据存储或公共 URL 中的单个或多个文件。 通过此方法可以下载文件或将其作为 FileDataset 对象装载到计算中。 文件可以采用任何格式，因此可以实现更广泛的机器学习方案，包括深度学习。

若要详细了解即将发生的 API 更改，请参阅[数据集 API 更改通知](https://aka.ms/tabular-dataset)。

## <a name="create-datasets"></a>创建数据集

创建数据集时，将会创建对数据源位置的引用及其元数据的副本。 由于数据保留在其现有位置，因此不会产生额外的存储成本。 可以使用 Python SDK 或工作区登陆页（预览版）创建 `TabularDataset` 和 `FileDataset` 数据集。

要使数据可供 Azure 机器学习访问，必须从 [Azure 数据存储](how-to-access-data.md)或公共 Web URL 中的路径创建数据集。

### <a name="use-the-sdk"></a>使用 SDK

若要使用 Python SDK 从 [Azure 数据存储](how-to-access-data.md)创建数据集：

1. 验证是否对已注册的 Azure 数据存储拥有 `contributor` 或 `owner` 访问权限。

1. 通过引用数据存储中的路径创建数据集：

    ```Python
    from azureml.core.workspace import Workspace
    from azureml.core.datastore import Datastore
    from azureml.core.dataset import Dataset
    
    datastore_name = 'your datastore name'
    
    # get existing workspace
    workspace = Workspace.from_config()
    
    # retrieve an existing datastore in the workspace by name
    datastore = Datastore.get(workspace, datastore_name)
    ```

#### <a name="create-a-tabulardataset"></a>创建 TabularDataset

可以通过 SDK 或使用 Azure 机器学习工作室创建 TabularDataset。 可以从数据中的列或者从数据所存储到的路径模式指定一个时间戳，以启用时序特征。 此规范允许按时间轻松有效地进行筛选。

使用 `TabularDatasetFactory` 类中的 [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header-true--partition-format-none-) 方法可以读取 .csv 或 .tsv 格式的文件，以及创建未注册的 TabularDataset。 如果从多个文件进行读取，结果将聚合为一种表格表示形式。

```Python
# create a TabularDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'weather/2018/11.csv'),
                  (datastore, 'weather/2018/12.csv'),
                  (datastore, 'weather/2019/*.csv')
                 ]
weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```

默认情况下，在创建 TabularDataset 时，将自动推断列数据类型。 如果推断出的类型与预期不符，可以使用以下代码指定列类型。 还可以[详细了解支持的数据类型](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.datatype?view=azure-ml-py)。

```Python
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |PassengerId|Survived|Pclass|名称|Sex|Age|SibSp|Parch|Ticket|Fare|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Mr. Owen Harris|男|22.0|1|0|A/5 21171|7.2500||S
1|2|True|1|Cumings, Mrs. John Bradley (Florence Briggs Th...|女|38.0|1|0|PC 17599|71.2833|C85|C
2|3|True|3|Heikkinen, Miss. Laina|女|26.0|0|0|STON/O2. 3101282|7.9250||S

使用 `TabularDatasetFactory` 类中的 [`from_sql_query()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-sql-query-query--validate-true--set-column-types-none-) 方法可从 Azure SQL 数据库进行读取：

```Python

from azureml.core import Dataset, Datastore

# create tabular dataset from a SQL database in datastore
sql_datastore = Datastore.get(workspace, 'mssql')
sql_ds = Dataset.Tabular.from_sql_query((sql_datastore, 'SELECT * FROM my_table'))
```

在 TabularDataset 中，可以从数据中的列或者从数据所存储到的任何路径模式指定一个时间戳，以启用时序特征。 此规范允许按时间轻松有效地进行筛选。

使用 `TabularDataset` 类中的 [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-fine-grain-timestamp--coarse-grain-timestamp-none--validate-false-) 方法可以指定时间戳列并启用按时间筛选。 有关详细信息，请参阅[使用 NOAA 天气数据的表格时序相关 API 演示](https://aka.ms/azureml-tsd-notebook)。

```Python
# create a TabularDataset with time series trait
datastore_paths = [(datastore, 'weather/*/*/*/data.parquet')]

# get a coarse timestamp column from the path pattern
dataset = Dataset.Tabular.from_parquet_files(path=datastore_path, partition_format='weather/{coarse_time:yyy/MM/dd}/data.parquet')

# set coarse timestamp to the virtual column created, and fine grain timestamp from a column in the data
dataset = dataset.with_timestamp_columns(fine_grain_timestamp='datetime', coarse_grain_timestamp='coarse_time')

# filter with time-series-trait-specific methods
data_slice = dataset.time_before(datetime(2019, 1, 1))
data_slice = dataset.time_after(datetime(2019, 1, 1))
data_slice = dataset.time_between(datetime(2019, 1, 1), datetime(2019, 2, 1))
data_slice = dataset.time_recent(timedelta(weeks=1, days=1))
```

#### <a name="create-a-filedataset"></a>创建 FileDataset

使用 `FileDatasetFactory` 类中的 [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) 方法可以加载任意格式的文件并创建未注册的 FileDataset：

```Python
# create a FileDataset pointing to files in 'animals' folder and its subfolders recursively
datastore_paths = [
                  (datastore, 'animals')
                 ]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = [
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz'
           ]
mnist_ds = Dataset.File.from_files(path=web_paths)
```

#### <a name="on-the-web"></a>在 Web 上 
以下步骤和动画演示如何在 Azure 机器学习工作室 (https://ml.azure.com ) 中创建数据集。

![使用 UI 创建数据集](./media/how-to-create-register-datasets/create-dataset-ui.gif)

若要在工作室中创建数据集：
1. 在 https://ml.azure.com 上登录。
1. 在左侧窗格的“资产”部分，选择“数据集”。   
1. 选择“创建数据集”以选择数据集的源。  此源可以是本地文件、数据存储或公共 URL。
1. 为“数据集类型”选择“表格”或“文件”。  
1. 选择“下一步”查看“设置和预览”、“架构”和“确认详细信息”窗体；其中的信息已根据文件类型智能填充。     使用这些窗体检查所做的选择，并在创建数据集之前进一步对其进行配置。  
1. 选择“创建”以完成数据集的创建。 

## <a name="register-datasets"></a>注册数据集

若要完成创建过程，请将数据集注册到工作区。 使用 [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) 方法将数据集注册到工作区，以便与其他人共享，并在不同的试验中重复使用这些数据集：

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

> [!Note]
> 通过 Azure 机器学习工作室创建的数据集会自动注册到工作区。

## <a name="create-datasets-with-azure-open-datasets"></a>使用 Azure 开放数据集创建数据集

[Azure 开放数据集](https://azure.microsoft.com/services/open-datasets/)是精选公共数据集，可用于将方案专属特征添加到机器学习解决方案，以提高模型的准确度。 数据集包括不受任何限制的天气、人口普查、节假日、公共安全和位置数据，有助于定型机器学习模型和扩充预测解决方案。 开放数据集位于 Microsoft Azure 云中，并且会同时包含在 SDK 和工作区 UI 中。

### <a name="use-the-sdk"></a>使用 SDK

若要通过 SDK 使用 Azure 开放数据集创建数据集，请确保已使用 `pip install azureml-opendatasets` 安装相应的包。 每个离散数据集在 SDK 中由其自身的类表示，某些类作为 `TabularDataset` 和/或 `FileDataset` 提供。 有关完整的类列表，请参阅[参考文档](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py)。

大多数类继承自 `TabularDataset` 的实例并返回该实例。 这些类的示例包括 `PublicHolidays`、`BostonSafety` 和 `UsPopulationZip`。 若要从这些类类型创建 `TabularDataset`，请使用不带参数的构造函数。 注册从开放数据集创建的数据集时，不会立即下载数据，但以后可按请求（例如，在训练期间）从某个中心存储位置访问数据。 

```python
from azureml.opendatasets import UsPopulationZip

tabular_dataset = UsPopulationZip()
tabular_dataset = tabular_dataset.register(workspace=workspace, name="pop data", description="US population data by zip code")
```

可以检索 `TabularDataset` 或 `FileDataset` 形式的某些类，这样就可以直接操控和/或下载文件。 其他类只能使用 `get_tabular_dataset()` 或 `get_file_dataset()` 函数获取数据集。 以下代码示例演示了上述类类型的几个例子：

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY return TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

### <a name="use-the-ui"></a>使用 UI

也可以通过 UI 从开放数据集类创建数据集。 在工作区中，选择“资产”下的“数据集”选项卡。   在“创建数据集”下拉菜单中，选择“从开放数据集”。  

![UI 中的开放数据集](./media/how-to-create-register-datasets/open-datasets-1.png)

选择数据集对应的磁贴将其选中。 （可以选择使用搜索栏进行筛选。）选择“**下一步**”。

![选择数据集](./media/how-to-create-register-datasets/open-datasets-2.png)

选择数据集的注册名称，并（可选）使用可用的筛选器筛选数据。 在本例中，对于公共节假日数据集，可按如下所述进行筛选：将时间段设置为一年，将国家/地区代码设置为仅限 US。 选择“创建”  。

![设置数据集参数并创建数据集](./media/how-to-create-register-datasets/open-datasets-3.png)

现在，该数据集已显示在工作区中的“数据集”下。  可以像使用创建的其他数据集一样来使用它。

## <a name="version-datasets"></a>对数据集进行版本控制

可以通过创建新版本，以相同的名称注册新数据集。 数据集版本是为数据状态设置书签的一种方法，以便可以应用数据集的特定版本进行试验或者在将来重现该数据集。 详细了解[数据集版本](how-to-version-track-datasets.md)。
```Python
# create a TabularDataset from Titanic training data
web_paths = [
            'https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
            'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv'
           ]
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```


## <a name="access-datasets-in-your-script"></a>在脚本中访问数据集

可以在本地以及在 Azure 机器学习计算等计算群集上远程访问已注册的数据集。 若要跨试验访问已注册的数据集，请使用以下代码按名称访问工作区和已注册的数据集。 默认情况下，`Dataset` 类中的 [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) 方法返回已注册到工作区的数据集的最新版本。

```Python
%%writefile $script_folder/train.py

from azureml.core import Dataset, Run

run = Run.get_context()
workspace = run.experiment.workspace

dataset_name = 'titanic_ds'

# Get a dataset by name
titanic_ds = Dataset.get_by_name(workspace=workspace, name=dataset_name)

# Load a TabularDataset into pandas DataFrame
df = titanic_ds.to_pandas_dataframe()
```

## <a name="next-steps"></a>后续步骤

* 了解[如何使用数据集进行训练](how-to-train-with-datasets.md)。
* 通过自动化机器学习[使用 TabularDataset 进行训练](https://aka.ms/automl-dataset)。
* 有关更多数据集训练示例，请参阅[示例笔记本](https://aka.ms/dataset-tutorial)。
