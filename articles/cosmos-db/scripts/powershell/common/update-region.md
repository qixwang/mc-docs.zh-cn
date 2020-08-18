---
title: PowerShell 脚本：更新 Azure Cosmos 帐户的区域
description: Azure PowerShell 脚本示例 - 更新 Azure Cosmos 帐户的区域
ms.service: cosmos-db
ms.topic: sample
origin.date: 05/01/2020
author: rockboyfor
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 3729d309e02a80308fcc0586045a5e49599e1a1e
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223584"
---
<!--Verified successfully-->
# <a name="update-an-azure-cosmos-accounts-regions-using-powershell"></a>使用 PowerShell 更新 Azure Cosmos 帐户的区域

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

> [!NOTE]
> 不能在同一操作中修改区域并更改其他 Cosmos 帐户属性。 这些操作必须以两个单独的操作来完成。
> [!NOTE]
> 此示例演示如何使用 SQL (Core) API 帐户。 若要将此示例用于其他 API，请复制相关属性，并将其应用于 API 特定的脚本。

```powershell
# Reference: Az.CosmosDB | https://docs.microsoft.com/powershell/module/az.cosmosdb
# --------------------------------------------------
# Purpose
# Update Cosmos DB account: Add an Azure region (location)
# NOTE: if adding a region to a single master account, do not change the first 
# region in the same operation. Change failover priority order in a
# separate operation.
# NOTE: this operation will return, but account updates may still be
# occurring. Check the account or Resource Group's activity log for status.
# --------------------------------------------------
# Variables - ***** SUBSTITUTE YOUR VALUES *****
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "myaccount" # Must be all lower case

# Regions ordered by failover priority, with each indicating whether AZ-enabled
# Region AZ status can be checked at https://azure.microsoft.com/global-infrastructure/regions/
$locations = @(
    @{name = "China East"; azEnabled = $true};
    @{name = "China North"; azEnabled = $false};
)
# --------------------------------------------------

Write-Host "Get Cosmos DB account"
$account = Get-AzCosmosDBAccount -ResourceGroupName $resourceGroupName -Name $accountName

$i = 0
$locationObjects = @()

ForEach ($location in $locations) {
    $locationObjects += New-AzCosmosDBLocationObject -LocationName $location.name -IsZoneRedundant $location.azEnabled -FailoverPriority ($i++)
}

Write-Host "Update Cosmos DB account region(s)"
Update-AzCosmosDBAccountRegion -InputObject $account -LocationObject $locationObjects

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
| [Get-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | 列出 Cosmos DB 帐户或获取指定的 Cosmos DB 帐户。 |
| [New-AzCosmosDBLocationObject](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdblocationobject) | 创建一个 PSLocation 类型对象，用作 Update-AzCosmosDBAccountRegion 的参数。 |
| [Update-AzCosmosDBAccountRegion](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbaccountregion) | 更新 Cosmos DB 帐户的区域。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: new article about update region -->
<!--NEW.date: 08/17/2020-->