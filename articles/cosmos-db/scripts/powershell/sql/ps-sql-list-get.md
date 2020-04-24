---
title: 用于列出和获取操作的 PowerShell 脚本 - Azure Cosmos DB SQL (Core) API
description: Azure PowerShell 脚本 - Azure Cosmos DB 列出和获取操作 - SQL (Core) API
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 05/18/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 7192fc31b0838f5c16427a10e16c06a9ac84005e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76270085"
---
# <a name="list-and-get-databases-and-containers-for-azure-cosmos-db---sql-core-api"></a>列出和获取 Azure Cosmos DB 的数据库和容器 - SQL (Core) API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# List and Get operations for Cosmos SQL API
$apiVersion = "2015-04-08"
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$accountResourceName = $accountName + "/sql/"
$databaseName = "database1"
$databaseResourceName = $accountName + "/sql/" + $databaseName
$databaseResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases"
$containerName = "container1"
$containerResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName
$containerResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers"

Read-Host -Prompt "List all databases in an account. Press Enter to continue"

Get-AzResource -ResourceType $databaseResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $accountResourceName  | Select-Object Properties

Read-Host -Prompt "Get a database in an account. Press Enter to continue"

Get-AzResource -ResourceType $databaseResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName | Select-Object Properties

Read-Host -Prompt "List all containers in a database. Press Enter to continue"

Get-AzResource -ResourceType $containerResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName | Select-Object Properties

Read-Host -Prompt "Get a container in a database. Press Enter to continue"

Get-AzResource -ResourceType $containerResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $containerResourceName | Select-Object Properties

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

<!--Update_Description: update meta properties -->