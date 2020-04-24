---
title: 更新 Azure Cosmos DB 的表 API 表的 RU/秒
description: 更新 Azure Cosmos DB 的表 API 表的 RU/秒
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
origin.date: 09/25/2019
ms.date: 10/28/2019
ms.openlocfilehash: e68fc4f887b67b40f69e26e987707cd0d3e734a3
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914778"
---
# <a name="update-rus-for-a-table-api-table-for-azure-cosmos-db-azure-cli"></a>更新 Azure Cosmos DB 的表 API 表的 RU/秒 Azure CLI

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题需要运行 Azure CLI 2.0.73 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

此脚本创建一个表 API 表，然后更新该表的吞吐量。

```azurecli
!/bin/bash

# Update throughput for Table API table

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Variables for Cassandra API resources
resourceGroupName="Group-$uniqueId"
location='chinanorth2'
accountName="cosmos-$uniqueId" #needs to be lower case
tableName='table1'

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a Cosmos account for Table API
az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --capabilities EnableTable \

# Create a Table API Table
az cosmosdb table create \
    -a $accountName \
    -g $resourceGroupName \
    -n $tableName \
    --throughput 400

read -p 'Press any key to increase Table throughput to 500'

az cosmosdb table throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -n $tableName \
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
| [az cosmosdb table create](https://docs.azure.cn/cli/cosmosdb/table?view=azure-cli-latest#az-cosmosdb-table-create) | 创建 Azure Cosmos 表 API 表。 |
| [az cosmosdb table throughput update](https://docs.azure.cn/cli/cosmosdb/table/throughput?view=azure-cli-latest#az-cosmosdb-table-throughput-update) | 更新 Azure Cosmos 表 API 表的 RU/秒。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure Cosmos DB CLI 的详细信息，请参阅 [Azure Cosmos DB CLI 文档](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)。

可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 脚本示例。

<!--Verify successfully-->
<!--Update_Description: new articles on cosmos db table throughput with cli  -->
<!--New.date: 10/28/2019-->