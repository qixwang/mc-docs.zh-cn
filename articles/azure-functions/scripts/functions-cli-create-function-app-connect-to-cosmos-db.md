---
title: 创建连接到 Azure Cosmos DB 的函数应用 - Azure CLI
description: Azure CLI 脚本示例 - 创建用于连接到 Azure Cosmos DB 的 Azure Function
ms.topic: sample
ms.date: 02/14/2020
ms.custom: mvc
ms.openlocfilehash: f47eb42d2da7506d0fa8603154f204ef0ec51319
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428687"
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>创建用于连接到 Azure Cosmos DB 的 Azure Function

此 Azure Functions 示例脚本先创建一个函数应用，然后将该函数连接到 Azure Cosmos DB 数据库。 创建的应用设置（包含连接）可以与 [Azure Cosmos DB 触发器或绑定](../functions-bindings-cosmosdb.md)配合使用。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

如果在本地使用 CLI，请确保运行 Azure CLI 2.0 或更高版本。 若要查找版本，请运行 `az --version`。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

此示例创建 Azure Function app，并将 Cosmos DB 终结点和访问密钥添加到应用设置。

```azurecli
#!/bin/bash

# create a resource group with location
az group create `
  --name myResourceGroup `
  --location chinanorth

# create a storage account 
az storage account create `
  --name funccosmosdbstore `
  --location chinanorth `
  --resource-group myResourceGroup `
  --sku Standard_LRS

# create an app service plan
az appservice plan create `
  --name myappserviceplan `
  --resource-group myResourceGroup `
  --location chinanorth

# create a new function app, assign it to the resource group you have just created
az functionapp create `
  --name myfunccosmosdb `
  --resource-group myResourceGroup `
  --storage-account funccosmosdbstore `
  --plan myappserviceplan

# create cosmosdb database, name must be lowercase.
az cosmosdb create `
  --name myfunccosmosdb `
  --resource-group myResourceGroup

# Retrieve cosmosdb connection string
$endpoint=az cosmosdb show --name myfunccosmosdb --resource-group myResourceGroup --query documentEndpoint --output tsv

$key=az cosmosdb list-keys --name myfunccosmosdb --resource-group myResourceGroup --query primaryMasterKey --output tsv

# configure function app settings to use cosmosdb connection string
az functionapp config appsettings set `
  --name myfunccosmosdb `
  --resource-group myResourceGroup `
  --setting CosmosDB_Endpoint=$endpoint CosmosDB_Key=$key
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令：表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az-group-create) | 使用相关位置创建资源组 |
| [az storage accounts create](/cli/storage/account#az-storage-account-create) | 创建存储帐户 |
| [az functionapp create](/cli/functionapp#az-functionapp-create) | 在无服务器[消耗计划](../functions-scale.md#consumption-plan)中创建函数应用。 |
| [az cosmosdb create](/cli/cosmosdb#az-cosmosdb-create) | 创建 Azure Cosmos DB 数据库。 |
| [az cosmosdb show](/cli/cosmosdb#az-cosmosdb-show)| 获取数据库帐户连接。 |
| [az cosmosdb list-keys](/cli/cosmosdb#az-cosmosdb-list-keys)| 获取数据库密钥。 |
| [az functionapp config appsettings set](/cli/functionapp/config/appsettings#az-functionapp-config-appsettings-set) | 将连接字符串设置为函数应用中的应用设置。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可以在 [Azure Functions 文档](../functions-cli-samples.md)中找到其他 Azure Functions CLI 脚本示例。


<!-- Update_Description: wording update -->



