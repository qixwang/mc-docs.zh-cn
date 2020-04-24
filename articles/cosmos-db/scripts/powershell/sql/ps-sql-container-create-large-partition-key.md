---
title: 使用大分区键创建 Azure Cosmos 容器的 PowerShell 脚本
description: Azure PowerShell 脚本示例 - 在 Azure Cosmos 帐户中创建具有大分区键的容器
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 07/03/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 765969fe07469eb03d43d2279563f98e59603180
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76270086"
---
# <a name="create-a-container-with-a-large-partition-key-in-an-azure-cosmos-account-using-powershell"></a>使用 PowerShell 在 Azure Cosmos 帐户中创建具有大分区键的容器

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Create a Cosmos SQL API container with large partition key support (version 2)

#generate a random 10 character alphanumeric string to ensure unique resource names
$uniqueId=$(-join ((97..122) + (48..57) | Get-Random -Count 15 | % {[char]$_}))

$apiVersion = "2015-04-08"
$location = "China North 2"
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount-$uniqueId" # must be lower case.
$databaseName = "database1"
$containerName = "container1"
$databaseResourceName = $accountName + "/sql/" + $databaseName
$containerResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName
$accountResourceType = "Microsoft.DocumentDb/databaseAccounts"
$databaseResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases"
$containerResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers"

$locations = @(
    @{ "locationName"="China North 2"; "failoverPriority"=0 },
    @{ "locationName"="China East 2"; "failoverPriority"=1 }
)

$accountProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations
}

New-AzResource -ResourceType $accountResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $accountProperties

#Database
$databaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"= 400 }
} 
New-AzResource -ResourceType $databaseResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName -PropertyObject $databaseProperties

# Container with large partition key support (version = 2)
$containerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash";
            "version" = 2
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent"; 
            "includedPaths"= @(@{
                "path"="/*"
            });
            "excludedPaths"= @(@{
                "path"="/myPathToNotIndex/*"
            })
        }
    }
} 

New-AzResource -ResourceType $containerResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $containerResourceName -PropertyObject $containerProperties

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
