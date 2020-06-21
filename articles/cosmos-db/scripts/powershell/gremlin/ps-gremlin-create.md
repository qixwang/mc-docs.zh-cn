---
title: 用于创建 Azure Cosmos DB Gremlin API 数据库和图的 PowerShell 脚本
description: Azure PowerShell 脚本 - Azure Cosmos DB 创建 Gremlin API 数据库和图
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
origin.date: 05/13/2020
ms.date: 06/22/2020
ms.author: v-yeche
ms.openlocfilehash: a90d3ef11faf8552fecfc996e08ecf1fad12d981
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85098699"
---
# <a name="create-a-database-and-graph-for-azure-cosmos-db---gremlin-api"></a>创建 Azure Cosmos DB 的数据库和图 - Gremlin API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Reference: Az.CosmosDB | https://docs.microsoft.com/powershell/module/az.cosmosdb
# --------------------------------------------------
# Purpose
# Create Cosmos Gremlin API account, database, and graph
# with dedicated throughput and conflict resolution policy
# with last writer wins and custom resolver path
# --------------------------------------------------
Function New-RandomString{Param ([Int]$Length = 10) return $(-join ((97..122) + (48..57) | Get-Random -Count $Length | ForEach-Object {[char]$_}))}
# --------------------------------------------------
$uniqueId = New-RandomString -Length 7 # Random alphanumeric string for unique resource names
$apiKind = "Gremlin"
# --------------------------------------------------
# Variables - ***** SUBSTITUTE YOUR VALUES *****
$locations = @("China East", "China North") # Regions ordered by failover priority
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "cosmos-$uniqueId" # Must be all lower case
$consistencyLevel = "Session"
$tags = @{Tag1 = "MyTag1"; Tag2 = "MyTag2"; Tag3 = "MyTag3"}
$databaseName = "myDatabase"
$graphName = "myGraph"
$graphRUs = 400
$partitionKeys = @("/myPartitionKey")
# --------------------------------------------------
$conflictResolutionPath = "/_ts"
# --------------------------------------------------
Write-Host "Creating account $accountName"
$account = New-AzCosmosDBAccount -ResourceGroupName $resourceGroupName `
    -Location $locations -Name $accountName -ApiKind $apiKind -Tag $tags `
    -DefaultConsistencyLevel $consistencyLevel `
    -EnableAutomaticFailover:$true

Write-Host "Creating database $databaseName"
$database = New-AzCosmosDBGremlinDatabase -ParentObject $account `
    -Name $databaseName

# Prepare conflict resolution policy object for graph
$conflictResolutionPolicy = New-AzCosmosDBGremlinConflictResolutionPolicy `
    -Type LastWriterWins -Path $conflictResolutionPath

Write-Host "Creating graph $graphName"
$graph = New-AzCosmosDBGremlinGraph -ParentObject $database `
    -Name $graphName -Throughput $graphRUs `
    -PartitionKeyKind Hash -PartitionKeyPath $partitionKeys `
    -ConflictResolutionPolicy $conflictResolutionPolicy

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
| [New-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | 创建 Cosmos DB 帐户。 |
| [New-AzCosmosDBGremlinDatabase](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbgremlindatabase) | 创建 Gremlin API 数据库。 |
| [New-AzCosmosDBGremlinConflictResolutionPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbgremlinconflictresolutionpolicy) | 创建 Gremlin API 写入冲突解决策略。 |
| [New-AzCosmosDBGremlinGraph](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbgremlingraph) | 创建 Gremlin API 图。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->