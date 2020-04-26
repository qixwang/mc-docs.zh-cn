---
title: 将 BACPAC 文件导入 Azure SQL 数据库的 CLI 示例
description: 用于将 BACPAC 文件导入 SQL 数据库的 Azure CLI 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: load & move data
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 05/24/2019
ms.date: 04/27/2020
ms.openlocfilehash: 85e9894930a20169d2809447799549ddef6e9d36
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127236"
---
# <a name="use-cli-to-import-a-bacpac-file-into-an-azure-sql-database"></a>使用 CLI 将 BACPAC 文件导入 Azure SQL 数据库

以下 Azure CLI 脚本示例将数据库从 *.bacpac* 文件导入 Azure SQL 数据库。  

本文要求运行 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>运行脚本

```azurecli
#!/bin/bash
location="China East"
randomIdentifier=random123

resource="resource-$randomIdentifier"
server="server-$randomIdentifier"
database="database-$randomIdentifier"
storage="storage$randomIdentifier"
container="container-$randomIdentifier"

bacpac="sample.bacpac"

login="sampleLogin"
password="samplePassword123!"

echo "Using resource group $resource with login: $login, password: $password..."

echo "Creating $resource..."
az group create --name $resource --location "$location"

echo "Creating $storage..."
az storage account create --name $storage --resource-group $resource --location "$location" --sku Standard_LRS

echo "Creating $container on $storage..."
key=$(az storage account keys list --account-name $storage --resource-group $resource -o json --query [0].value | tr -d '"')

az storage container create --name $container --account-key $key --account-name $storage #--public-access container

echo "Downloading sample database..."
az rest --uri https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bacpac --output-file $bacpac -m get --skip-authorization-header

echo "Uploading sample database to $container..."
az storage blob upload --container-name $container --file $bacpac --name $bacpac --account-key $key --account-name $storage

echo "Creating $server..."
az sql server create --name $server --resource-group $resource --location "$location" --admin-user $login --admin-password $password
az sql server firewall-rule create --resource-group $resource --server $server --name AllowAzureServices --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0

echo "Creating $database..."
az sql db create --name $database --resource-group $resource --server $server --edition "GeneralPurpose"

echo "Importing sample database from $container to $database..."
az sql db import --admin-password $password --admin-user $login --storage-key $key --storage-key-type StorageAccessKey --storage-uri https://$storage.blob.core.chinacloudapi.cn/$container/$bacpac --name $database --resource-group $resource --server $server
```

### <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```azurecli
az group delete --name $resource
```

## <a name="sample-reference"></a>示例参考

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| | |
|---|---|
| [az sql server](/cli/sql/server) | 服务器命令。 |
| [az sql db import](/cli/sql/db#az-sql-db-import) | 数据库导入命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
