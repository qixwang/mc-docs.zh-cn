---
title: CLI 示例 - 活动异地复制 - 单一 Azure SQL 数据库
description: Azure CLI 示例脚本，用于为 Azure SQL 数据库中的单一数据库设置活动异地复制并进行故障转移。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 03/12/2019
ms.date: 04/27/2020
ms.openlocfilehash: fba2d6f09471ba73e6e383f2606b0a9c9f481afd
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127251"
---
# <a name="use-cli-to-configure-active-geo-replication-for-a-single-database-in-azure-sql-database"></a>使用 CLI 为 Azure SQL 数据库中的单一数据库配置活动异地复制

此 Azure CLI 脚本示例为单一数据库配置活动异地复制，并将其故障转移到数据库的辅助副本。

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

secondaryResource="secondaryresource-$randomIdentifier"
secondaryLocation="China North"
secondaryServer="secondaryserver-$randomIdentifier"

login="sampleLogin"
password="samplePassword123!"

echo "Using resource group $resource with login: $login, password: $password..."

echo "Creating $resource and $secondaryResource..."
az group create --name $resource --location "$location"
az group create --name $secondaryResource --location "$secondaryLocation"

echo "Creating $server in $location and $secondaryServer in $secondaryLocation..."
az sql server create --name $server --resource-group $resource --location "$location" --admin-user $login --admin-password $password
az sql server create --name $secondaryServer --resource-group $secondaryResource --location "$secondaryLocation" --admin-user $login --admin-password $password

echo "Creating $database on $server..."
az sql db create --name $database --resource-group $resource --server $server --service-objective S0

echo "Establishing geo-replication on $database..."
az sql db replica create --name $database --partner-server $secondaryServer --resource-group $resource --server $server --partner-resource-group $secondaryResource
az sql db replica list-links --name $database --resource-group $resource --server $server

echo "Initiating failover..."
az sql db replica set-primary --name $database --resource-group $secondaryResource --server $secondaryServer

echo "Monitoring health of $database..."
az sql db replica list-links --name $database --resource-group $secondaryResource --server $secondaryServer

echo "Removing replication link after failover..."
az sql db replica delete-link --resource-group $secondaryResource --server $secondaryServer --name $database --partner-server $server --yes
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
| [az sql db replica](/cli/sql/db/replica) | 数据库副本命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。