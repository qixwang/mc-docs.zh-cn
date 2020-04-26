---
title: CLI 示例 - 故障转移组 - Azure SQL 数据库弹性池
description: Azure CLI 示例脚本，用于创建 Azure SQL 数据库弹性池，将其添加到故障转移组，然后测试故障转移。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 07/16/2019
ms.date: 04/27/2020
ms.openlocfilehash: 3f14ae3de156e8d4476646a8ae8ff2477378c173
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127247"
---
# <a name="use-cli-to-add-an-azure-sql-database-elastic-pool-to-a-failover-group"></a>使用 CLI 将 Azure SQL 数据库弹性池添加到故障转移组

此 Azure CLI 脚本示例创建一个单一数据库，将其添加到弹性池，创建一个故障转移组，然后测试故障转移。

本文要求运行 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>运行脚本

```azurecli
#!/bin/bash
location="China East"
randomIdentifier=random123

resource="resource-$randomIdentifier"
server="sqlserver-$randomIdentifier"
pool="pool-$randomIdentifier"
database="database-$randomIdentifier"

failover="failover-$randomIdentifier"
failoverLocation="China North"
failoverServer="sqlsecondary-$randomIdentifier"

login="sampleLogin"
password="samplePassword123!"

echo "Using resource group $resource with login: $login, password: $password..."

echo "Creating $resource..."
az group create --name $resource --location "$location"

echo "Creating $server in $location..."
az sql server create --name $server --resource-group $resource --location "$location"  --admin-user $login --admin-password $password

echo "Creating $database on $server..."
az sql db create --name $database --resource-group $resource --server $server --sample-name AdventureWorksLT

echo "Creating $pool on $server..."
az sql elastic-pool create --name $pool --resource-group $resource --server $server

echo "Adding $database to $pool..."
az sql db update --elastic-pool $pool --name $database --resource-group $resource --server $server

echo "Creating $failoverServer in $failoverLocation..."
az sql server create --name $failoverServer --resource-group $resource --location "$failoverLocation"  --admin-user $login --admin-password $password

echo "Creating $pool on $failoverServer..."
az sql elastic-pool create --name $pool --resource-group $resource --server $failoverServer

echo "Creating $failover between $server and $failoverServer..."
az sql failover-group create --name $failover --partner-server $failoverServer --resource-group $resource --server $server --failover-policy Automatic --grace-period 2

databaseId=$(az sql elastic-pool list-dbs --name $pool --resource-group $resource --server $server --query [0].name -o json | tr -d '"')

echo "Adding $database to $failover..."
az sql failover-group update --name $failover --add-db $databaseId --resource-group $resource --server $server

echo "Confirming role of $failoverServer is secondary..." # note ReplicationRole property
az sql failover-group show --name $failover --resource-group $resource --server $server

echo "Failing over to $failoverServer..."
az sql failover-group set-primary --name $failover --resource-group $resource --server $failoverServer 

echo "Confirming role of $failoverServer is now primary..." # note ReplicationRole property
az sql failover-group show --name $failover --resource-group $resource --server $server

echo "Failing back to $server...."
az sql failover-group set-primary --name $failover --resource-group $resource --server $server

```

### <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```azurecli
az group delete --name $resource
```

## <a name="sample-reference"></a>示例参考

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| | |
|---|---|
| [az sql elastic-pool](/cli/sql/elastic-pool) | 弹性池命令。 |
| [az sql failover-group ](/cli/sql/failover-group) | 故障转移组命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview)。

可以在 [Azure SQL 数据库 Azure CLI 脚本](../sql-database-cli-samples.md)中找到其他 SQL 数据库 Azure CLI 脚本示例。
