---
title: PowerShell 脚本：创建 Azure Cosmos DB Cassandra API 密钥空间和表
description: Azure PowerShell 脚本 - Azure Cosmos DB 创建 Cassandra API 密钥空间和表
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
origin.date: 03/18/2020
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: 84a77ae42cd7f1f215cf42e6ec4bc868ebfe6c20
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134579"
---
<!--Verify successfully-->
# <a name="create-a-keyspace-and-table-for-azure-cosmos-db---cassandra-api"></a>创建 Azure Cosmos DB 的密钥空间和表 - Cassandra API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Reference: Az.CosmosDB | https://docs.microsoft.com/powershell/module/az.cosmosdb
# --------------------------------------------------
# Purpose
# Create Cosmos Cassandra API account with automatic failover,
# a keyspace, and a table with defined schema, dedicated throughput, and
# conflict resolution policy with last writer wins and custom resolver path.
# --------------------------------------------------
Function New-RandomString{Param ([Int]$Length = 10) return $(-join ((97..122) + (48..57) | Get-Random -Count $Length | ForEach-Object {[char]$_}))}
# --------------------------------------------------
$uniqueId = New-RandomString -Length 7 # Random alphanumeric string for unique resource names
$apiKind = "Cassandra"
# --------------------------------------------------
# Variables - ***** SUBSTITUTE YOUR VALUES *****
$locations = @("China East", "China North") # Regions ordered by failover priority
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "cosmos-$uniqueId" # Must be all lower case
$consistencyLevel = "BoundedStaleness"
$maxStalenessInterval = 300
$maxStalenessPrefix = 100000
$tags = @{Tag1 = "MyTag1"; Tag2 = "MyTag2"; Tag3 = "MyTag3"}
$keyspaceName = "mykeyspace"
$tableName = "mytable"
$tableRUs = 400
$partitionKeys = @("machine", "cpu", "mtime")
$clusterKeys = @( 
    @{ name = "loadid"; orderBy = "Asc" };
    @{ name = "duration"; orderBy = "Desc" }
)
$columns = @(
    @{ name = "loadid"; type = "uuid" };
    @{ name = "machine"; type = "uuid" };
    @{ name = "cpu"; type = "int" };
    @{ name = "mtime"; type = "int" };
    @{ name = "load"; type = "float" };
    @{ name = "duration"; type = "float" }
)
# --------------------------------------------------
# Account
Write-Host "Creating account $accountName"
# Cassandra not yet supported in New-AzCosmosDBAccount
# $account = New-AzCosmosDBAccount -ResourceGroupName $resourceGroupName `
    # -Location $locations -Name $accountName -ApiKind $apiKind -Tag $tags `
    # -DefaultConsistencyLevel $consistencyLevel `
    # -MaxStalenessIntervalInSeconds $maxStalenessInterval `
    # -MaxStalenessPrefix $maxStalenessPrefix `
    # -EnableAutomaticFailover:$true
# Account creation: use New-AzResource with property object
# --------------------------------------------------
$azAccountResourceType = "Microsoft.DocumentDb/databaseAccounts"
$azApiVersion = "2020-03-01"
$azApiType = "EnableCassandra"

$azLocations = @()
$i = 0
ForEach ($location in $locations) {
    $azLocations += @{ locationName = "$location"; failoverPriority = $i++ }
}

$azConsistencyPolicy = @{
    defaultConsistencyLevel = $consistencyLevel;
    maxIntervalInSeconds = $maxStalenessInterval;
    maxStalenessPrefix = $maxStalenessPrefix;
}

$azAccountProperties = @{
    capabilities = @( @{ name = $azApiType; } );
    databaseAccountOfferType = "Standard";
    locations = $azLocations;
    consistencyPolicy = $azConsistencyPolicy;
    enableAutomaticFailover = "true";
}

New-AzResource -ResourceType $azAccountResourceType -ApiVersion $azApiVersion `
    -ResourceGroupName $resourceGroupName -Location $locations[0] `
    -Name $accountName -PropertyObject $azAccountProperties `
    -Tag $tags -Force

$account = Get-AzCosmosDBAccount -ResourceGroupName $resourceGroupName -Name $accountName

Write-Host "Creating keyspace $keyspaceName"
$keyspace = Set-AzCosmosDBCassandraKeyspace -InputObject $account `
    -Name $keyspaceName

# Table Schema
$psClusterKeys = @()
ForEach ($clusterKey in $clusterKeys) {
    $psClusterKeys += New-AzCosmosDBCassandraClusterKey -Name $clusterKey.name -OrderBy $clusterKey.orderBy
}

$psColumns = @()
ForEach ($column in $columns) {
    $psColumns += New-AzCosmosDBCassandraColumn -Name $column.name -Type $column.type
}

$schema = New-AzCosmosDBCassandraSchema `
    -PartitionKey $partitionKeys `
    -ClusterKey $psClusterKeys `
    -Column $psColumns

Write-Host "Creating table $tableName"
$table = Set-AzCosmosDBCassandraTable -InputObject $keyspace `
    -Name $tableName -Schema $schema -Throughput $tableRUs 

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
|**Azure 资源**| |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | 创建资源。 |
|**Azure Cosmos DB**| |
| [Set-AzCosmosDBCassandraKeyspace](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbcassandrakeyspace) | 创建或更新 Cosmos DB Cassandra API 密钥空间。 |
| [New-AzCosmosDBCassandraClusterKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandraclusterkey) | 新建 CosmosDB Cassandra 群集密钥。 |
| [New-AzCosmosDBCassandraColumn](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandracolumn) | 新建 CosmosDB Cassandra 列。 |
| [New-AzCosmosDBCassandraSchema](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandraschema) | 新建 CosmosDB Cassandra 架构。 |
| [Set-AzCosmosDBCassandraTable](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbcassandratable) | 创建或更新 Cosmos DB Cassandra API 表。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->
