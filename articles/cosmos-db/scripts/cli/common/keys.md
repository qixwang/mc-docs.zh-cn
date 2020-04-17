---
title: 使用 Azure Cosmos 帐户的帐户密钥和连接字符串
description: 使用 Azure Cosmos 帐户的帐户密钥和连接字符串
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 09/25/2019
ms.date: 10/28/2019
ms.openlocfilehash: f338565a8b55cb6f6e1b9493beab93a1a43629b4
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914856"
---
<!--Verify successfully-->
# <a name="work-with-account-keys-and-connection-strings-for-an-azure-cosmos-account-using-azure-cli"></a>通过 Azure CLI 使用 Azure Cosmos 帐户的帐户密钥和连接字符串

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题需要运行 Azure CLI 2.0.73 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

此脚本演示四个操作。

- 列出所有帐户密钥
- 列出只读帐户密钥
- 列出连接字符串
- 重新生成帐户密钥

> [!NOTE]
> 此示例演示了如何使用 SQL (Core) API 帐户，但帐户密钥和连接字符串操作在 Cosmos DB 中的所有数据库 API 中都是相同的。

```azurecli
#!/bin/bash

# This sample shows the following:
# List all account keys
# List read only account keys
# List connection strings
# Regenerate account keys

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Resource group and Cosmos account variables
resourceGroupName="Group-$uniqueId"
location='chinanorth2'
accountName="cosmos-$uniqueId" #needs to be lower case

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a Cosmos DB account with default values
# Use appropriate values for --kind or --capabilities for other APIs
az cosmosdb create -n $accountName -g $resourceGroupName

read -p "Press any key to list account keys"
# List all account keys
az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName

read -p "Press any key to list read only account keys"
# List read-only keys
az cosmosdb list-read-only-keys \
    -n $accountName \
    -g $resourceGroupName

read -p "Press any key to list connection strings"
# List connection strings
az cosmosdb list-connection-strings \
    -n $accountName \
    -g $resourceGroupName

read -p "Press any key to regenerate secondary account keys"
# Regenerate secondary account keys
# key-kind values: primary, primaryReadonly, secondary, secondaryReadonly
az cosmosdb regenerate-key \
    -n $accountName \
    -g $resourceGroupName \
    --key-kind secondary

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb create](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-create) | 创建 Azure Cosmos DB 帐户。 |
| [az cosmosdb keys list](https://docs.azure.cn/cli/cosmosdb/keys?view=azure-cli-latest#az-cosmosdb-keys-list) | 列出 Azure Cosmos DB 帐户的密钥。 |
| [az cosmosdb list-read-only-keys](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-list-read-only-keys) | 列出 Azure Cosmos DB 帐户的只读密钥。 |
| [az cosmosdb list-connection-strings](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-list-connection-strings) | 列出 Azure Cosmos DB 帐户的连接字符串。 |
| [az cosmosdb regenerate-key](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-regenerate-key) | 重新生成 Azure Cosmos DB 帐户的密钥。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure Cosmos DB CLI 的详细信息，请参阅 [Azure Cosmos DB CLI 文档](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)。

可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 脚本示例。

<!--Update_Description: new articles on cosmos db common keys -->
<!--New.date: 10/28/2019-->