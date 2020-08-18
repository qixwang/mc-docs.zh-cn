---
title: 用于获取吞吐量（RU/秒）的 PowerShell 脚本 - Azure Cosmos DB 表 API
description: Azure PowerShell 脚本 - Azure Cosmos DB 获取吞吐量（RU/秒）- 表 API
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
origin.date: 03/18/2020
author: rockboyfor
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: c07f2b652ba94d93f8ad2ff3d466236ba46c7bce
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223553"
---
<!--Verified successfully-->
# <a name="get-throughput-rus-for-a-table-for-azure-cosmos-db---table-api"></a>获取 Azure Cosmos DB 的表的吞吐量（RU/秒）- 表 API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Reference: Az.CosmosDB | https://docs.microsoft.com/powershell/module/az.cosmosdb
# --------------------------------------------------
# Purpose
# Get RU/s throughput for Azure Cosmos DB Table API table
# --------------------------------------------------
# Variables - ***** SUBSTITUTE YOUR VALUES *****
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "myaccount" # Must be all lower case
$tableName = "myTable"
# --------------------------------------------------

Get-AzCosmosDBTableThroughput -ResourceGroupName $resourceGroupName `
    -AccountName $accountName -Name $tableName

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
| [Get-AzCosmosDBTableThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbtablethroughput) | 获取指定表 API 表的吞吐量值。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: new article about throughput get -->
<!--NEW.date: 08/17/2020-->