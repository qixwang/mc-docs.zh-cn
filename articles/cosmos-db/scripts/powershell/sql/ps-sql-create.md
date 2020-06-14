---
title: 用于创建 Azure Cosmos DB SQL API 数据库和容器的 PowerShell 脚本
description: Azure PowerShell 脚本 - Azure Cosmos DB 创建 SQL API 数据库和容器
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 05/01/2020
ms.date: 06/15/2020
ms.author: v-yeche
ms.openlocfilehash: 33e466db9687c4ff511fc7f8c8c938e9e9b6ff7a
ms.sourcegitcommit: 3de7d92ac955272fd140ec47b3a0a7b1e287ca14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723623"
---
# <a name="create-a-database-and-container-for-azure-cosmos-db---sql-api"></a>创建 Azure Cosmos DB 的数据库和容器 - SQL API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

此脚本在两个具有会话级一致性的区域、一个数据库和一个具有分区键、自定义索引策略、唯一键策略、TTL、专用吞吐量和“以最后一次写入为准”冲突解决策略的容器中使用在 `multipleWriteLocations=true` 时将使用的自定义冲突解决路径为 SQL (Core) API 创建 Cosmos 帐户。

```powershell
# Reference: Az.CosmosDB | https://docs.microsoft.com/powershell/module/az.cosmosdb
# --------------------------------------------------
# Purpose
# Create Cosmos SQL API account, database, and container with dedicated throughput,
# indexing policy with include, exclude, and composite paths, unique key, and conflict resolution
# --------------------------------------------------
Function New-RandomString{Param ([Int]$Length = 10) return $(-join ((97..122) + (48..57) | Get-Random -Count $Length | ForEach-Object {[char]$_}))}
# --------------------------------------------------
$uniqueId = New-RandomString -Length 7 # Random alphanumeric string for unique resource names
$apiKind = "Sql"
# --------------------------------------------------
# Variables - ***** SUBSTITUTE YOUR VALUES *****
$locations = @("China East", "China North") # Regions ordered by failover priority
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "cosmos-$uniqueId" # Must be all lower case
$consistencyLevel = "Session"
$tags = @{Tag1 = "MyTag1"; Tag2 = "MyTag2"; Tag3 = "MyTag3"}
$databaseName = "myDatabase"
$containerName = "myContainer"
$containerRUs = 400
$partitionKeyPath = "/myPartitionKey"
$indexPathIncluded = "/*"
$compositeIndexPaths1 = @(
    @{ Path = "/myCompositePath1"; Order = "ascending" };
    @{ Path = "/myCompositePath2"; Order = "descending" }
)
$compositeIndexPaths2 = @(
    @{ Path = "/myCompositePath3"; Order = "ascending" };
    @{ Path = "/myCompositePath4"; Order = "descending" }
)
$indexPathExcluded = "/myExcludedPath/*"
$uniqueKeyPath = "/myUniqueKeyPath"
$conflictResolutionPath = "/myResolutionPath"
$ttlInSeconds = 120 # Set this to -1 (or don't use it at all) to never expire
# --------------------------------------------------
Write-Host "Creating account $accountName"
$account = New-AzCosmosDBAccount -ResourceGroupName $resourceGroupName `
    -Location $locations -Name $accountName -ApiKind $apiKind -Tag $tags `
    -DefaultConsistencyLevel $consistencyLevel `
    -EnableAutomaticFailover:$true

Write-Host "Creating database $databaseName"
$database = New-AzCosmosDBSqlDatabase -ParentObject $account -Name $databaseName

$uniqueKey = New-AzCosmosDBSqlUniqueKey -Path $uniqueKeyPath
$uniqueKeyPolicy = New-AzCosmosDBSqlUniqueKeyPolicy -UniqueKey $uniqueKey

$compositePath1 = @()
ForEach ($compositeIndexPath in $compositeIndexPaths1) {
    $compositePath1 += New-AzCosmosDBSqlCompositePath `
        -Path $compositeIndexPath.Path `
        -Order $compositeIndexPath.Order
}

$compositePath2 = @()
ForEach ($compositeIndexPath in $compositeIndexPaths2) {
    $compositePath2 += New-AzCosmosDBSqlCompositePath `
        -Path $compositeIndexPath.Path `
        -Order $compositeIndexPath.Order
}

$includedPathIndex = New-AzCosmosDBSqlIncludedPathIndex -DataType String -Kind Range
$includedPath = New-AzCosmosDBSqlIncludedPath -Path $indexPathIncluded -Index $includedPathIndex

$indexingPolicy = New-AzCosmosDBSqlIndexingPolicy `
    -IncludedPath $includedPath `
    -CompositePath @($compositePath1, $compositePath2) `
    -ExcludedPath $indexPathExcluded `
    -IndexingMode Consistent -Automatic $true

# Conflict resolution policies only apply in multi-master accounts.
# Included here to show custom resolution path.
$conflictResolutionPolicy = New-AzCosmosDBSqlConflictResolutionPolicy `
    -Type LastWriterWins -Path $conflictResolutionPath

Write-Host "Creating container $containerName"
$container = New-AzCosmosDBSqlContainer `
    -ParentObject $database -Name $containerName `
    -Throughput $containerRUs -IndexingPolicy $indexingPolicy `
    -PartitionKeyKind Hash -PartitionKeyPath $partitionKeyPath `
    -UniqueKeyPolicy $uniqueKeyPolicy `
    -ConflictResolutionPolicy $conflictResolutionPolicy `
    -TtlInSeconds $ttlInSeconds

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
|**Azure Cosmos DB**| |
| [New-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | 新建 Cosmos DB 帐户。 |
| [Set-AzCosmosDBSqlDatabase](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbsqldatabase) | 新建或更新现有 Cosmos DB SQL 数据库。 |
| [New-AzCosmosDBSqlUniqueKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekey) | 新建一个 Cosmos DB SQL PSSqlUniqueKey 对象，用作 New-AzCosmosDBSqlUniqueKeyPolicy 的参数。 |
| [New-AzCosmosDBSqlUniqueKeyPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekeypolicy) | 新建一个 Cosmos DB SQL PSSqlUniqueKeyPolicy 对象，用作 Set-AzCosmosDBSqlContainer 的参数。 |
| [New-AzCosmosDBSqlCompositePath](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlcompositepath) | 新建一个 Cosmos DB SQL PSCompositePath 对象，用作 New-AzCosmosDBSqlIndexingPolicy 的参数。 |
| [New-AzCosmosDBSqlIncludedPathIndex](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpathindex) | 创建一个类型为 PSIndexes 的新对象，用作 Set-AzCosmosDBSqlIncludedPath 的参数。 |
| [New-AzCosmosDBSqlIncludedPath](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpath) | 创建一个类型为 PSIncludedPath 的新对象，用作 New-AzCosmosDBSqlIndexingPolicy 的参数。 |
| [New-AzCosmosDBSqlIndexingPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlindexingpolicy) | 创建一个类型为 PSSqlIndexingPolicy 的新对象，用作 Set-AzCosmosDBSqlContainer 的参数。 |
| [New-AzCosmosDBSqlConflictResolutionPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlconflictresolutionpolicy) | 创建一个类型为 PSSqlConflictResolutionPolicy 的新对象，用作 Set-AzCosmosDBSqlContainer 的参数。 |
| [Set-AzCosmosDBSqlContainer](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbsqlcontainer) | 新建或更新现有 Cosmos DB SQL 容器。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->