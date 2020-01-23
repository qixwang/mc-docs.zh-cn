---
title: 用于更新 Azure Cosmos DB Cassandra API 资源的 RU/秒的 PowerShell 脚本
description: 了解如何使用 PowerShell 脚本更新 Azure Cosmos DB Cassandra API 中密钥空间或表的吞吐量
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
origin.date: 12/05/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: ff491775b1846ec38df1f4d6e641a36be9a492ef
ms.sourcegitcommit: 304861faf39689348962127b8b56db8082ece2ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2020
ms.locfileid: "76270101"
---
# <a name="update-rus-for-a-keyspace-or-table-for-azure-cosmos-db---cassandra-api"></a>更新 Azure Cosmos DB 的密钥空间或表的 RU/秒 - Cassandra API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Update RU for an Azure Cosmos Cassandra API keyspace or table

$apiVersion = "2015-04-08"
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$keyspaceName = "keyspace1"
$tableName = "table1"
$keyspaceThroughputResourceName = $accountName + "/cassandra/" + $keyspaceName + "/throughput"
$keyspaceThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/keyspaces/settings"
$tableThroughputResourceName = $accountName + "/cassandra/" + $keyspaceName + "/" + $tableName + "/throughput"
$tableThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/keyspaces/tables/settings"
$throughput = 500
$updateResource = "keyspace" # or "table"

$properties = @{
    "resource"=@{"throughput"=$throughput}
}

if($updateResource -eq "keyspace"){
    Set-AzResource -ResourceType $keyspaceThroughputResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $keyspaceThroughputResourceName -PropertyObject $properties
}
elseif($updateResource -eq "table"){
Set-AzResource -ResourceType $tableThroughputResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $tableThroughputResourceName -PropertyObject $tableProperties
}
else {
    Write-Host("Must select keyspace or table")    
}

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
|**Azure 资源**| |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | 创建资源。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!--Update_Description: update meta properties -->
