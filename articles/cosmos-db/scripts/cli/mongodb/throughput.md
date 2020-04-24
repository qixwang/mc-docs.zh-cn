---
title: 更新 MongoDB API for Azure Cosmos DB 的数据库和集合的 RU/秒
description: 更新 MongoDB API for Azure Cosmos DB 的数据库和集合的 RU/秒
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
origin.date: 09/25/2019
ms.date: 10/28/2019
ms.openlocfilehash: dc63c885e5d6799de3be82331dea0a26ffc73cf3
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914782"
---
# <a name="update-rus-for-a-database-and-collection-for-mongodb-api-for-azure-cosmos-db-using-azure-cli"></a>使用 Azure CLI 更新 MongoDB API for Azure Cosmos DB 的数据库和集合的 RU/秒

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题需要运行 Azure CLI 2.0.73 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

此脚本为 Azure Cosmos DB for MongoDB API 创建具有共享吞吐量的数据库和具有专用吞吐量的集合，然后更新数据库和集合的吞吐量。

```azurecli
!/bin/bash

# Update throughput for MongoDB API database and collection

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Variables for MongoDB API resources
resourceGroupName="Group-$uniqueId"
location='chinanorth2'
accountName="cosmos-$uniqueId" #needs to be lower case
databaseName='database1'
collectionName='collection1'

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a Cosmos account for MongoDB API
az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --kind MongoDB

# Create a MongoDB API database with shared throughput
az cosmosdb mongodb database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput 400

# Define a minimal index policy for the collection
idxpolicy=$(cat << EOF 
[ 
    {"key": {"keys": ["user_id"]}}
]
EOF
)
# Persist index policy to json file
echo "$idxpolicy" > "idxpolicy-$uniqueId.json"

# Create a MongoDB API collection
az cosmosdb mongodb collection create \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $collectionName \
    --shard 'user_id' \
    --throughput 400 \
    --idx @idxpolicy-$uniqueId.json

# Clean up temporary index policy file
rm -f "idxpolicy-$uniqueId.json"

read -p 'Press any key to increase Database throughput to 500'

az cosmosdb mongodb database throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput 500

read -p 'Press any key to increase Collection throughput to 500'

az cosmosdb mongodb collection throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $collectionName \
    --throughput 500

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb create](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-create) | 创建 Azure Cosmos DB 帐户。 |
| [az cosmosdb mongodb database create](https://docs.azure.cn/cli/cosmosdb/mongodb/database?view=azure-cli-latest#az-cosmosdb-mongodb-database-create) | 创建 Azure Cosmos MongoDB API 数据库。 |
| [az cosmosdb mongodb collection create](https://docs.azure.cn/cli/cosmosdb/mongodb/collection?view=azure-cli-latest#az-cosmosdb-mongodb-collection-create) | 创建 Azure Cosmos MongoDB API 集合。 |
| [az cosmosdb mongodb database throughput update](https://docs.azure.cn/cli/cosmosdb/mongodb/database/throughput?view=azure-cli-latest#az-cosmosdb-mongodb-database-throughput-update) | 更新 Azure Cosmos MongoDB API 数据库的 RU。 |
| [az cosmosdb mongodb collection throughput update](https://docs.azure.cn/cli/cosmosdb/mongodb/collection/throughput?view=azure-cli-latest#az-cosmosdb-mongodb-collection-throughput-update) | 更新 Azure Cosmos MongoDB API 集合的 RU。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure Cosmos DB CLI 的详细信息，请参阅 [Azure Cosmos DB CLI 文档](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)。

可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 脚本示例。

<!--Verify successfully-->
<!--Update_Description: new articles on mongodb throughput -->
<!--New.date: 10/28/2019-->