---
title: 创建 Azure Cosmos DB 的 Cassandra 密钥空间和表
description: 创建 Azure Cosmos DB 的 Cassandra 密钥空间和表
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
origin.date: 09/25/2019
ms.date: 10/28/2019
ms.openlocfilehash: 590040131a8506fdab7ed12fd616b6f1a5fc5e35
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914849"
---
<!--Verify successfully-->
# <a name="create-an-azure-cosmos-cassandra-api-account-keyspace-and-table-using-azure-cli"></a>使用 Azure CLI 创建 Azure Cosmos Cassandra API 帐户、密钥空间和表

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题需要运行 Azure CLI 2.0.73 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

```azurecli
!/bin/bash

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Create a Cassandra keyspace and table

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Variables for Cassandra API resources
resourceGroupName="Group-$uniqueId"
location='chinanorth2'
accountName="cosmos-$uniqueId" #needs to be lower case
keySpaceName='keyspace1'
tableName='table1'

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a Cosmos account for Cassandra API
az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --capabilities EnableCassandra \
    --default-consistency-level Eventual \
    --locations regionName='China North 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='China East 2' failoverPriority=1 isZoneRedundant=False

# Create a Cassandra Keyspace
az cosmosdb cassandra keyspace create \
    -a $accountName \
    -g $resourceGroupName \
    -n $keySpaceName

# Define the schema for the table
schema=$(cat << EOF 
{
    "columns": [
        {"name": "columnA","type": "uuid"},
        {"name": "columnB","type": "int"},
        {"name": "columnC","type": "text"}
    ],
    "partitionKeys": [
        {"name": "columnA"}
    ],
    "clusterKeys": [
        { "name": "columnB", "orderBy": "asc" }
    ]
}
EOF
)
# Persist schema to json file
echo "$schema" > "schema-$uniqueId.json"

# Create the Cassandra table
az cosmosdb cassandra table create \
    -a $accountName \
    -g $resourceGroupName \
    -k $keySpaceName \
    -n $tableName \
    --throughput 400 \
    --schema @schema-$uniqueId.json

# Clean up temporary schema file
rm -f "schema-$uniqueId.json"

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
| [az cosmosdb cassandra keyspace create](https://docs.azure.cn/cli/cosmosdb/cassandra/keyspace?view=azure-cli-latest#az-cosmosdb-cassandra-keyspace-create) | 创建 Azure Cosmos Cassandra 密钥空间。 |
| [az cosmosdb cassandra table create](https://docs.azure.cn/cli/cosmosdb/cassandra/table?view=azure-cli-latest#az-cosmosdb-cassandra-table-create) | 创建 Azure Cosmos Cassandra 表。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure Cosmos DB CLI 的详细信息，请参阅 [Azure Cosmos DB CLI 文档](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)。

可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 脚本示例。

<!--Update_Description: new articles on cassandra create with cli -->
<!--New.date: 10/28/2019-->