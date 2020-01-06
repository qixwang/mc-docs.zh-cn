---
title: 创建 Azure Data Lake Storage Gen2 存储帐户 | Microsoft Docs
description: 快速学习使用 Azure 门户、Azure PowerShell 或 Azure CLI 创建能够访问 Data Lake Storage Gen2 的新存储帐户。
author: WenJason
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
origin.date: 10/23/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.reviewer: stewu
ms.openlocfilehash: 188d7c2e36259d1fb40bd9bcfef0ecd8da5f0f1e
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624280"
---
# <a name="create-an-azure-data-lake-storage-gen2-storage-account"></a>创建 Azure Data Lake Storage Gen2 存储帐户

Azure Data Lake Storage Gen2 [支持分层命名空间](data-lake-storage-introduction.md)，该命名空间提供了一个适合与 Hadoop 分布式文件系统 (HDFS) 配合使用的基于本机目录的容器。 可以通过 [ABFS 驱动程序](data-lake-storage-abfs-driver.md)从 HDFS 访问 Data Lake Storage Gen2 数据。

本文演示了如何使用 Azure 门户、Azure PowerShell 或通过 Azure CLI 创建帐户。

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。 

|           | 先决条件 |
|-----------|--------------|
|门户     | 无         |
|PowerShell | 本文需要 PowerShell 模块 Az.Storage 0.7 或更高版本  。 若要查找当前版本，请运行 `Get-Module -ListAvailable Az.Storage` 命令。 如果在运行此命令后，没有显示任何结果，或者如果出现 0.7 以下的版本，则必须升级 powershell 模块  。 请参阅本指南的[升级 powershell 模块](#upgrade-your-powershell-module)部分。
|CLI        | 可以安装 CLI 并在本地运行 CLI 命令。|

### <a name="install-the-cli-locally"></a>在本地安装 CLI

可在本地安装和使用 Azure CLI。 本文要求运行 Azure CLI 2.0.38 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="create-a-storage-account-with-azure-data-lake-storage-gen2-enabled"></a>创建启用了 Azure Data Lake Storage Gen2 的存储帐户

Azure 存储帐户包含所有的 Azure 存储数据对象：Blob、文件、队列、表和磁盘。 存储帐户为你的 Azure 存储数据提供了一个唯一的命名空间，可以从世界上的任何位置通过 HTTP 或 HTTPS 访问该命名空间。 Azure 存储帐户中的数据是持久的，高度可用、安全且可大规模缩放。

> [!NOTE]
> 必须将新的存储帐户创建为 **StorageV2(常规用途 v2 )** 类型才能利用 Data Lake Storage Gen2 功能。  

有关存储帐户的详细信息，请参阅 [Azure 存储帐户概述](../common/storage-account-overview.md)。

## <a name="create-an-account-using-the-azure-portal"></a>使用 Azure 门户创建帐户

登录到 [Azure 门户](https://portal.azure.cn)。

### <a name="create-a-storage-account"></a>创建存储帐户

每个存储帐户都必须属于 Azure 资源组。 资源组是对 Azure 资源进行分组的逻辑容器。 在创建存储帐户时，可以选择创建新的资源组，也可以使用现有资源组。 本文介绍如何创建新资源组。

若要在 Azure 门户中创建常规用途 v2 存储帐户，请执行以下步骤：

> [!NOTE]
> 分层命名空间目前在所有公共区域中提供。

1. 选择要在其中创建存储帐户的订阅。
2. 在 Azure 门户中，选择“创建资源”  按钮，然后选择“存储帐户”  。
3. 在“资源组”  字段下，选择“新建”  。 输入新资源组的名称。
   
   资源组是对 Azure 资源进行分组的逻辑容器。 在创建存储帐户时，可以选择创建新的资源组，也可以使用现有资源组。

4. 然后，输入存储帐户的名称。 所选名称在 Azure 中必须唯一。 该名称还必须为 3 到 24 个字符，并且只能包含数字和小写字母。
5. 选择一个位置。
6. 确保“StorageV2 (常规用途 v2)”  在“帐户类型”  下拉列表中显示为选中状态。
7. （可选）更改以下每个字段中的值：**性能**、**复制**、**访问层**。 若要详细了解这些选项，请参阅 [Azure 存储简介](/storage/common/storage-introduction#azure-storage-services)。
8. 选择“高级”选项卡  。
10. 在“Data Lake Storage Gen2”  部分中，将“分层命名空间”设置为“已启用”。  
11. 单击“查看 + 创建”以创建存储帐户  。

现在已通过门户创建了存储帐户。

### <a name="clean-up-resources"></a>清理资源

若要使用 Azure 门户删除资源组，请执行以下操作：

1. 在 Azure 门户中展开左侧的菜单，打开服务菜单，然后选择“资源组”以显示资源组的列表。 
2. 找到要删除的资源组，右键单击列表右侧的“更多”按钮 ( **...** )。 
3. 选择“删除资源组”并进行确认。 

## <a name="create-an-account-using-powershell"></a>使用 PowerShell 创建帐户

首先，安装最新版本的 [PowerShellGet](https://docs.microsoft.com/powershell/scripting/gallery/installing-psget) 模块。

然后，升级 powershell 模块，登录到 Azure 订阅，创建资源组，然后创建存储帐户。

### <a name="upgrade-your-powershell-module"></a>升级 powershell 模块

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

若要使用 PowerShell 与 Data Lake Storage Gen2 交互，需要安装模块 Az.Storage 0.7 或更高版本  。

首先使用提升的权限打开 PowerShell 会话。

安装 Az.Storage 模块

```powershell
Install-Module Az.Storage -Repository PSGallery -AllowClobber -Force
```

### <a name="sign-in-to-your-azure-subscription"></a>登录到 Azure 订阅

使用 `Login-AzAccount` 命令并按照屏幕上的说明进行身份验证。

```powershell
Login-AzAccount -EnvironmentName AzureChinaCloud
```

### <a name="create-a-resource-group"></a>创建资源组

若要通过 PowerShell 创建新的资源组，请使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 命令： 

> [!NOTE]
> 分层命名空间目前在所有公共区域中提供。

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-quickstart-resource-group"
$location = "chinaeast"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-general-purpose-v2-storage-account"></a>创建常规用途 v2 存储帐户

若要使用本地冗余存储 (LRS) 从 PowerShell 创建常规用途 v2 存储帐户，请使用 [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/New-azStorageAccount) 命令：

```powershell
$location = "chinaeast"

New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name "storagequickstart" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2 `
  -EnableHierarchicalNamespace $True
```

### <a name="clean-up-resources"></a>清理资源

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) 命令： 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="create-an-account-using-azure-cli"></a>使用 Azure CLI 创建帐户

若要登录到本地安装的 CLI，请运行登录命令：

```cli
az login
```

### <a name="add-the-cli-extension-for-azure-data-lake-gen-2"></a>为 Azure Data Lake Gen 2 添加 CLI 扩展

若要使用 CLI 来与 Data Lake Storage Gen2 交互，必须将扩展添加到 shell。

为此，请使用本地 shell 输入以下命令：`az extension add --name storage-preview`

### <a name="create-a-resource-group"></a>创建资源组

若要通过 Azure CLI 创建新的资源组，请使用 [az group create](/cli/group) 命令。

```azurecli
az group create `
    --name storage-quickstart-resource-group `
    --location chinaeast
```

> [!NOTE]
> > 分层命名空间目前在所有公共区域中提供。

### <a name="create-a-general-purpose-v2-storage-account"></a>创建常规用途 v2 存储帐户

若要使用本地冗余存储从 Azure CLI 创建常规用途 v2 存储帐户，请使用 [az storage account create](/cli/storage/account) 命令。

```azurecli
az storage account create `
    --name storagequickstart `
    --resource-group storage-quickstart-resource-group `
    --location chinaeast `
    --sku Standard_LRS `
    --kind StorageV2 `
    --enable-hierarchical-namespace true
```

### <a name="clean-up-resources"></a>清理资源

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [az group delete](/cli/group) 命令。

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本文中，你已创建了一个具有 Data Lake Storage Gen2 功能的存储帐户。 若要了解如何通过存储帐户上传和下载 Blob，请参阅以下主题。

* [AzCopy V10](/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
