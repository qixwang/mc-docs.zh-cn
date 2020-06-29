---
title: 为 Azure Cosmos DB 表 API 表创建资源锁
description: 为 Azure Cosmos DB 表 API 表创建资源锁
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
origin.date: 06/03/2020
ms.date: 06/22/2020
ms.author: v-yeche
ms.openlocfilehash: dcf7f2fe81518016afd452ba9aa0a03d9e779472
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85102096"
---
<!--Verified successfully-->
# <a name="create-resource-lock-for-a-azure-cosmos-db-table-api-table-using-azure-cli"></a>使用 Azure CLI 为 Azure Cosmos DB 表 API 表创建资源锁

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

选择在本地安装并使用 CLI 时，本主题要求运行 Azure CLI 2.6.0 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

<!--CORRECT ON When you choose to install-->

> [!IMPORTANT]
> 除非在启用了 `disableKeyBasedMetadataWriteAccess` 属性的情况下先锁定 Cosmos DB 帐户，否则资源锁对通过 Cosmos DB 表 SDK、Azure 存储表 SDK 以及任何工具（通过帐户密钥连接）或 Azure 门户进行连接的用户所做的更改均无效。 要详细了解如何启用此属性，请参阅[防止 SDK 更改](../../../role-based-access-control.md#preventing-changes-from-cosmos-sdk)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash
resourceGroupName='myResourceGroup'
accountName='my-cosmos-account'
tableName='myTable'

lockType='CanNotDelete' # CanNotDelete or ReadOnly
tableParent="databaseAccounts/$accountName"
tableResourceType="$nameSpace/tables"
tableLockName='$tableName-Lock'

# Create a delete lock on table
az lock create --name $tableLockName \
    --resource-group $resourceGroupName \
    --resource-type $tableResourceType \
    --lock-type $lockType \
    --parent $tableParent \
    --resource $tableName

# List all locks on a Cosmos account
az lock list --resource-group $resourceGroupName \
    --resource-name $accountName \
    --namespace Microsoft.DocumentDB \
    --resource-type databaseAccounts

# Delete lock on table
lockid=$(az lock show --name $tableLockName \
        --resource-group $resourceGroupName \
        --resource-type $tableResourceType \
        --resource $tableName --parent $tableParent \
        --output tsv --query id)
az lock delete --ids $lockid

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az lock create](https://docs.azure.cn/cli/lock?view=azure-cli-latest#az-lock-create) | 创建锁。 |
| [az lock list](https://docs.azure.cn/cli/lock?view=azure-cli-latest#az-lock-list) | 列出锁信息。 |
| [az lock show](https://docs.azure.cn/cli/lock?view=azure-cli-latest#az-lock-show) | 显示锁的属性。 |
| [az lock delete](https://docs.azure.cn/cli/lock?view=azure-cli-latest#az-lock-delete) | 删除锁。 |

## <a name="next-steps"></a>后续步骤

-[锁定资源以防止意外更改](../../../../azure-resource-manager/management/lock-resources.md)

-[Azure Cosmos DB CLI 文档](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)。

-[Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)。

<!-- Update_Description: new article about lock -->
<!--NEW.date: 06/22/2020-->