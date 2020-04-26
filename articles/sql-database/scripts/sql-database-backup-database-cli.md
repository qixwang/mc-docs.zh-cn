---
title: CLI 示例 - 备份 Azure SQL 数据库
description: 用于将 Azure SQL 单一数据库备份到 Azure 存储容器的 Azure CLI 示例脚本
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 03/27/2019
ms.date: 04/27/2020
ms.openlocfilehash: 0f1b4e920d8084dd749aaa035ebd7d55b9777077
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127242"
---
# <a name="use-cli-to-backup-an-azure-sql-single-database-to-an-azure-storage-container"></a>使用 CLI 将 Azure SQL 单一数据库备份到 Azure 存储容器

此 Azure CLI 示例将 Azure SQL 数据库备份到 Azure 存储容器。  

本文要求运行 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>运行脚本

```azurecli
#!/bin/bash
location="China East"
randomIdentifier=random123

resource=resource-$randomIdentifier
server=server-$randomIdentifier
database=database-$randomIdentifier
storage=storage$randomIdentifier
container=container-$randomIdentifier

bacpac=backup.bacpac

login=sampleLogin
password=samplePassword123!

echo "Using resource group $resource with login: $login, password: $password..."

echo "Creating resource group $resource..."
az group create --name $resource --location "$location"

echo "Creating $storage..."
az storage account create --name $storage --resource-group $resource --location "$location" --sku Standard_LRS

echo "Creating $container on $storage..."
key=$(az storage account keys list --account-name $storage --resource-group $resource -o json --query [0].value | tr -d '"')
az storage container create --name $container --account-key $key --account-name $storage

echo "Creating $server..."
az sql server create --name $server --resource-group $resource --location "$location" --admin-user $login --admin-password $password
az sql server firewall-rule create --resource-group $resource --server $server --name AllowAzureServices --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0

echo "Creating $database..."
az sql db create --name $database --resource-group $resource --server $server --edition GeneralPurpose --sample-name AdventureWorksLT

echo "Backing up $database..."
az sql db export --admin-password $password --admin-user $login --storage-key $key --storage-key-type StorageAccessKey --storage-uri "https://$storage.blob.core.chinacloudapi.cn/$container/$bacpac" --name $database --resource-group $resource --server $server
```

### <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```azurecli
az group delete --name $resource
```

## <a name="sample-reference"></a>示例参考

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az sql server](/cli/sql/server) | 服务器命令。 |
| [az sql db](/cli/sql/db) | 数据库命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
