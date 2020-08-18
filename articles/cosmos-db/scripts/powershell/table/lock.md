---
title: 用于为 Azure Cosmos 表 API 表创建资源锁的 PowerShell 脚本
description: 为 Azure Cosmos 表 API 表创建资源锁
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
origin.date: 06/12/2020
author: rockboyfor
ms.date: 08/17/2020
ms.testscope: yes|no
ms.testdate: 08/17/2020null
ms.author: v-yeche
ms.openlocfilehash: 4d6589ba5381c9b110e0d5eab375cf96bc112f35
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223554"
---
<!--Verified successfully-->
# <a name="create-a-resource-lock-for-azure-cosmos-table-api-table-using-azure-powershell"></a>使用 Azure PowerShell 为 Azure Cosmos 表 API 表创建资源锁

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-PowerShell-install](../../../../../includes/sample-PowerShell-install-no-ssh.md)]

> [!IMPORTANT]
> 除非在启用了 `disableKeyBasedMetadataWriteAccess` 属性的情况下先锁定 Cosmos DB 帐户，否则资源锁对使用任何 Cosmos DB SDK、任何工具（通过帐户密钥连接）或 Azure 门户进行连接的用户所作的更改均无效。 要详细了解如何启用此属性，请参阅[防止 SDK 更改](../../../role-based-access-control.md#prevent-sdk-changes)。

## <a name="sample-script"></a>示例脚本

```powershell
# References:
# Az.CosmosDB | https://docs.microsoft.com/powershell/module/az.cosmosdb
# Az.Resources | https://docs.microsoft.com/powershell/module/az.resources
# --------------------------------------------------
# Purpose
# 
# --------------------------------------------------
# Variables
# ***** SUBSTITUTE YOUR VALUES *****
$resourceGroupName = "myResourceGroup"
$accountName = "myaccount"
$databaseName = "myDatabase"
# *****
$apiVersion = "2020-04-01" # Cosmos DB RP API version
$lockLevel = "CanNotDelete" # CanNotDelete or ReadOnly

$resourceTypeAccount = "Microsoft.DocumentDB/databaseAccounts"
$resourceTypeDatabase = "$resourceTypeAccount/tables"

$resourceNameDatabase = "$accountName/$databaseName"
$lockNameDatabase = "$accountName-$databaseName-Lock"
# --------------------------------------------------

Write-Host "Create a $lockLevel lock on resource $resourceNameDatabase"
New-AzResourceLock `
    -ApiVersion $apiVersion `
    -ResourceGroupName $resourceGroupName `
    -ResourceType $resourceTypeDatabase `
    -ResourceName $resourceNameDatabase `
    -LockName $lockNameDatabase `
    -LockLevel $lockLevel `
    -Force

Write-Host "List all locks on  Cosmos DB account $accountName to confirm lock creation"
Get-AzResourceLock `
    -ApiVersion $apiVersion `
    -ResourceGroupName $resourceGroupName `
    -ResourceType $resourceTypeAccount `
    -ResourceName $accountName

Write-Host "Delete lock on resource $resourceNameDatabase"
Remove-AzResourceLock `
    -ApiVersion $apiVersion `
    -ResourceGroupName $resourceGroupName `
    -ResourceType $resourceTypeDatabase `
    -ResourceName $resourceNameDatabase `
    -LockName $lockNameDatabase `
    -Force

Write-Host "List all locks on Cosmos DB account $accountName to confirm lock removal"
Get-AzResourceLock `
    -ApiVersion $apiVersion `
    -ResourceGroupName $resourceGroupName `
    -ResourceType $resourceTypeAccount `
    -ResourceName $accountName
```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```PowerShell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
|**Azure 资源**| |
| [New-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcelock) | 创建资源锁。 |
| [Get-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcelock) | 获取资源锁，或列出资源锁。 |
| [Remove-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcelock) | 删除资源锁。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../PowerShell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。
