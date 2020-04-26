---
title: CLI 示例 - 还原/备份 Azure SQL 数据库
description: Azure CLI 示例脚本，用于使用自动备份将 Azure SQL 单一数据库还原到更早的时间点
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 03/27/2019
ms.date: 04/27/2020
ms.openlocfilehash: bd080c8239ff00a84c9605dddddb519a20f79956
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127253"
---
# <a name="use-cli-to-restore-an-azure-sql-single-database-to-an-earlier-point-in-time"></a>使用 CLI 将 Azure SQL 单一数据库还原到更早的时间点

此 Azure CLI 脚本示例将 Azure SQL 数据库还原到特定的时间点。  

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

server="server-$randomIdentifier"
database="database-$randomIdentifier"
restore="restore-$randomIdentifier"

login="sampleLogin"
password="samplePassword123!"

echo "Using resource group $resource with login: $login, password: $password..."

echo "Creating $resource..."
az group create --name $resource --location "$location"

echo "Creating $server..."
az sql server create --name $server --resource-group $resource --location "$location" --admin-user $login --admin-password $password

echo "Creating $database on $server..."
az sql db create --resource-group $resource --server $server --name $database --service-objective S0

echo "Sleeping..."
sleep 960s
restoreDateTime=$(date +%s)
restoreDateTime=$(expr $restoreDateTime - 120)
restoreDateTime=$(date -d @$restoreDateTime +"%Y-%m-%dT%T")

echo "Restoring $database to $restoreDateTime..." # restore database to its state 2 minutes ago, point-in-time restore requires database to be at least 5 minutes old
az sql db restore --dest-name $restore --edition Standard --name $database --resource-group $resource --server $server --service-objective S0 --time $restoreDateTime
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
| [az sql db restore](/cli/sql/db#az-sql-db-restore) | 还原数据库命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
