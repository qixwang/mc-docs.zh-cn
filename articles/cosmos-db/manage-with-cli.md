---
title: 使用 Azure CLI 管理 Azure Cosmos DB 资源
description: 使用 Azure CLI 管理 Azure Cosmos DB 帐户、数据库和容器。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 01/21/2020
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: daa07bacb6bd94287bacad51c961959d604cce00
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028524"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>使用 Azure CLI 管理 Azure Cosmos 资源

以下指南介绍了使用 Azure CLI 自动管理 Azure Cosmos DB 帐户、数据库和容器的常见命令。 [Azure CLI 参考](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)中收录了所有 Azure Cosmos DB CLI 命令的参考页。 还可以在[针对 Azure Cosmos DB 的 Azure CLI 示例](cli-samples.md)中找到更多示例，包括如何为 MongoDB、Gremlin、Cassandra 和表 API 创建和管理 Cosmos DB 帐户、数据库和容器。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="create-an-azure-cosmos-db-account"></a>创建 Azure Cosmos DB 帐户

在“中国北部 2”和“中国东部 2”区域创建启用了 SQL API 和会话一致性的 Azure Cosmos DB 帐户：

> [!IMPORTANT]
> Azure Cosmos 帐户名称必须为小写且小于 31 个字符。

```azurecli
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount' #needs to be lower case and less than 31 characters

az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --default-consistency-level Session \
    --locations regionName='China North 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='China East 2' failoverPriority=1 isZoneRedundant=False
```

## <a name="add-or-remove-regions"></a>添加或删除区域

创建包含两个区域的 Azure Cosmos 帐户，添加一个区域，并删除一个区域。

> [!NOTE]
> 不能同时添加或删除区域 `locations` 并更改 Azure Cosmos 帐户的其他属性。 修改区域的操作必须作为单独的操作与任何其他对帐户资源的更改操作分开执行。
> [!NOTE]
> 此命令可添加和删除区域，但不可修改故障转移优先级或触发手动故障转移。 请参阅[设置故障转移优先级](#set-failover-priority)和[触发手动故障转移](#trigger-manual-failover)。

```azurecli
resourceGroupName = 'myResourceGroup'
accountName = 'mycosmosaccount' # must be lower case and <31 characters

# Create an account with 2 regions
az cosmosdb create --name $accountName --resource-group $resourceGroupName \
    --locations regionName= "China North 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName= "China East 2" failoverPriority=1 isZoneRedundant=False

# Add a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName \
    --locations regionName= "China North 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName= "China East 2" failoverPriority=1 isZoneRedundant=False \
    --locations regionName= "China East" failoverPriority=2 isZoneRedundant=False

# Remove a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName \
    --locations regionName= "China North 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName= "China East 2" failoverPriority=1 isZoneRedundant=False
```

## <a name="enable-multiple-write-regions"></a>启用多个写入区域

为 Cosmos 帐户启用多主数据库

```azurecli
# Update an Azure Cosmos account from single to multi-master
resourceGroupName = 'myResourceGroup'
accountName = 'mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

az cosmosdb update --ids $accountId --enable-multiple-write-locations true
```

## <a name="set-failover-priority"></a>设置故障转移优先级

为已为自动故障转移而配置的 Azure Cosmos 帐户设置故障转移优先级

```azurecli
# Assume region order is initially 'China North 2'=0 'China East 2'=1 'China East'=2 for account
resourceGroupName = 'myResourceGroup'
accountName = 'mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

# Make China East the next region to fail over to instead of China East 2
az cosmosdb failover-priority-change --ids $accountId \
    --failover-policies 'China North 2'=0 'China East'=1 'China East 2'=2
```

## <a name="enable-automatic-failover"></a>启用自动故障转移

```azurecli
# Enable automatic failover on an existing account
resourceGroupName = 'myResourceGroup'
accountName = 'mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

az cosmosdb update --ids $accountId --enable-automatic-failover true
```

## <a name="trigger-manual-failover"></a>触发手动故障转移

> [!CAUTION]
> 在 priority = 0 的情况下更改区域会为 Azure Cosmos 帐户触发手动故障转移。 任何其他优先级更改都不会触发故障转移。

```azurecli
# Assume region order is initially 'China North 2'=0 'China East 2'=1 'China East'=2 for account
resourceGroupName = 'myResourceGroup'
accountName = 'mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

# Trigger a manual failover to promote China East 2 as new write region
az cosmosdb failover-priority-change --ids $accountId \
    --failover-policies 'China East 2'=0 'China East'=1 'China North 2'=2
```

<a name="list-account-keys"></a>
## <a name="list-all-account-keys"></a>列出所有帐户密钥

获取 Cosmos 帐户的所有密钥。

```azurecli
# List all account keys
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
   -n $accountName \
   -g $resourceGroupName
```

## <a name="list-read-only-account-keys"></a>列出只读帐户密钥

获取 Cosmos 帐户的只读密钥。

```azurecli
# List read-only account keys
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type read-only-keys
```

## <a name="list-connection-strings"></a>列出连接字符串

获取 Cosmos 帐户的连接字符串。

```azurecli
# List connection strings
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type connection-strings
```

## <a name="regenerate-account-key"></a>重新生成帐户密钥

重新生成 Cosmos 帐户的新密钥。

```azurecli
# Regenerate secondary account keys
# key-kind values: primary, primaryReadonly, secondary, secondaryReadonly
az cosmosdb keys regenerate \
    -n $accountName \
    -g $resourceGroupName \
    --key-kind secondary
```

## <a name="create-a-database"></a>创建数据库

创建 Cosmos 数据库。

```azurecli
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'

az cosmosdb sql database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName
```

## <a name="create-a-database-with-shared-throughput"></a>创建具有共享吞吐量的数据库

创建具有共享吞吐量的 Cosmos 数据库。

```azurecli
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
throughput=400

az cosmosdb sql database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $throughput
```

## <a name="change-the-throughput-of-a-database"></a>更改数据库的吞吐量

将 Cosmos 数据库的吞吐量增加 1000 RU/s。

```azurecli
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
newRU=1000

# Get minimum throughput to make sure newRU is not lower than minRU
minRU=$(az cosmosdb sql database throughput show \
    -g $resourceGroupName -a $accountName -n $databaseName \
    --query resource.minimumThroughput -o tsv)

if [ $minRU -gt $newRU ]; then
    newRU=$minRU
fi

az cosmosdb sql database throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $newRU
```

## <a name="create-a-container"></a>创建容器

创建带有默认索引策略、分区键且 RU/s 为 400 的 Cosmos 容器。

```azurecli
# Create a SQL API container
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
partitionKey='/myPartitionKey'
throughput=400

az cosmosdb sql container create \
    -a $accountName -g $resourceGroupName \
    -d $databaseName -n $containerName \
    -p $partitionKey --throughput $throughput
```

## <a name="create-a-container-with-ttl"></a>创建带有 TTL 的容器

创建启用了 TTL 的 Cosmos 容器。

```azurecli
# Create an Azure Cosmos container with TTL of one day
resourceGroupName = 'myResourceGroup'
accountName = 'mycosmosaccount'
databaseName = 'database1'
containerName = 'container1'

az cosmosdb sql container update \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $containerName \
    --ttl = 86400
```

## <a name="create-a-container-with-a-custom-index-policy"></a>创建带有自定义索引策略的容器

创建带有自定义索引策略、空间索引、组合索引、分区键且 RU/s 为 400 的 Cosmos 容器。

```azurecli
# Create a SQL API container
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
partitionKey='/myPartitionKey'
throughput=400

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Define the index policy for the container, include spatial and composite indexes
idxpolicy=$(cat << EOF
{
    "indexingMode": "consistent",
    "includedPaths": [
        {"path": "/*"}
    ],
    "excludedPaths": [
        { "path": "/headquarters/employees/?"}
    ],
    "spatialIndexes": [
        {"path": "/*", "types": ["Point"]}
    ],
    "compositeIndexes":[
        [
            { "path":"/name", "order":"ascending" },
            { "path":"/age", "order":"descending" }
        ]
    ]
}
EOF
)
# Persist index policy to json file
echo "$idxpolicy" > "idxpolicy-$uniqueId.json"

az cosmosdb sql container create \
    -a $accountName -g $resourceGroupName \
    -d $databaseName -n $containerName \
    -p $partitionKey --throughput $throughput \
    --idx @idxpolicy-$uniqueId.json

# Clean up temporary index policy file
rm -f "idxpolicy-$uniqueId.json"
```

## <a name="change-the-throughput-of-a-container"></a>更改容器的吞吐量

将 Cosmos 容器的吞吐量增加 1000 RU/s。

```azurecli
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
newRU=1000

# Get minimum throughput to make sure newRU is not lower than minRU
minRU=$(az cosmosdb sql container throughput show \
    -g $resourceGroupName -a $accountName -d $databaseName \
    -n $containerName --query resource.minimumThroughput -o tsv)

if [ $minRU -gt $newRU ]; then
    newRU=$minRU
fi

az cosmosdb sql container throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $containerName \
    --throughput $newRU
```

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅：

- [安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)
- [Azure CLI 参考](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)
- [针对 Azure Cosmos DB 的其他 Azure CLI 示例](cli-samples.md)

<!-- Update_Description: update meta properties, wording update -->
