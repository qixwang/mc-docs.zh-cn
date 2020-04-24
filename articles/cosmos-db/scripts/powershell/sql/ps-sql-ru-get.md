---
title: 用于获取预配吞吐量（RU/秒）的 PowerShell 脚本 - Azure Cosmos DB SQL (Core) API
description: Azure PowerShell 脚本 - Azure Cosmos DB 获取预配吞吐量（RU/秒）- SQL (Core) API
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 07/03/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 3643cf8ccb54b0f6b6b50c74c50900a5e071e1fd
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76270105"
---
# <a name="get-the-provisioned-throughput-rus-for-a-database-or-container-for-azure-cosmos-db---sql-core-api"></a>获取 Azure Cosmos DB 的数据库或容器的预配吞吐量（RU/秒）- SQL (Core) API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Get RU for an Azure Cosmos SQL (Core) API database or container
$apiVersion = "2015-04-08"
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$databaseThroughputResourceName = $accountName + "/sql/" + $databaseName + "/throughput"
$databaseThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/settings"
$containerThroughputResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName + "/throughput"
$containerThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers/settings"

# Check if throughput is set at database level (returns RU/s or error)
Get-AzResource -ResourceType $databaseThroughputResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $databaseThroughputResourceName  | Select-Object Properties

# Check if throughput is set at container level (returns RU/s or error)
Get-AzResource -ResourceType $containerThroughputResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $containerThroughputResourceName  | Select-Object Properties

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
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: wording update -->
