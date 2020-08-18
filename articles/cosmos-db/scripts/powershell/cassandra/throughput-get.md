---
title: 用于获取 Azure Cosmos DB Cassandra API 资源的吞吐量（RU/秒）的 PowerShell 脚本
description: Azure PowerShell 脚本 - Azure Cosmos DB 获取吞吐量（RU/秒）- Cassandra API
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
origin.date: 03/18/2020
author: rockboyfor
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: 06/15/2020
ms.author: v-yeche
ms.openlocfilehash: d690d04a0f9e74a3d1d90a4ab934f4ffed2aa9ec
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223488"
---
<!--Verified successfully-->
# <a name="get-throughput-rus-for-a-keyspace-or-table-for-azure-cosmos-db---cassandra-api"></a>获取 Azure Cosmos DB 的密钥空间或表的吞吐量（RU/秒）- Cassandra API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Reference: Az.CosmosDB | https://docs.microsoft.com/powershell/module/az.cosmosdb
# --------------------------------------------------
# Purpose
# Get keyspace or table throughput
# --------------------------------------------------
# Variables - ***** SUBSTITUTE YOUR VALUES *****
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "myaccount" # Must be all lower case
$keyspaceName = "mykeyspace" # Keyspace with shared throughput
$tableName = "mytable" # Table with dedicated throughput
# --------------------------------------------------

Write-Host "Get keyspace shared throughput"
Get-AzCosmosDBCassandraKeyspaceThroughput -ResourceGroupName $resourceGroupName `
    -AccountName $accountName -Name $keyspaceName

Write-Host "Get table dedicated throughput"
Get-AzCosmosDBCassandraTableThroughput -ResourceGroupName $resourceGroupName `
    -AccountName $accountName -KeyspaceName $keyspaceName `
    -Name $tableName

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
| [Get-AzCosmosDBCassandraKeyspaceThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbcassandrakeyspacethroughput) | 获取 Cassandra API 密钥空间的吞吐量值。 |
| [Get-AzCosmosDBCassandraTableThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbcassandratablethroughput) | 获取 Cassandra API 表的吞吐量值。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: new article about throughput get -->
<!--NEW.date: 08/17/2020-->