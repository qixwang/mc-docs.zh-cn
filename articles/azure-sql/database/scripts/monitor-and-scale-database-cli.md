---
title: Azure CLI：监视和缩放 Azure SQL 数据库中的单一数据库
description: 使用 Azure CLI 示例脚本监视和缩放 Azure SQL 数据库中的单一数据库。
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 06/25/2019
ms.date: 07/13/2020
ms.openlocfilehash: 6234e61de0a92bef766ac51ecf34e24932f4025e
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227734"
---
# <a name="use-the-azure-cli-to-monitor-and-scale-a-single-database-in-azure-sql-database"></a>使用 CLI 监视和缩放 Azure SQL 数据库中的单一数据库

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

此 Azure CLI 脚本示例在查询数据库的大小信息后，将 Azure SQL 数据库中的单一数据库缩放为不同的计算大小。

本文要求运行 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

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

login="sampleLogin"
password="samplePassword123!"

echo "Using resource group $resource with login: $login, password: $password..."

echo "Creating $resource..."
az group create --name $resource --location "$location"

echo "Creating $server on $resource..."
az sql server create --name $server --resource-group $resource --location "$location" --admin-user $login --admin-password $password

echo "Creating $database on $server..."
az sql db create --resource-group $resource --server $server --name $database --edition GeneralPurpose --family Gen4 --capacity 1 

echo "Monitoring size of $database..."
az sql db list-usages --name $database --resource-group $resource --server $server

echo "Scaling up $database..." # create command executes update if database already exists
az sql db create --resource-group $resource --server $server --name $database --edition GeneralPurpose --family Gen4 --capacity 2
```

> [!TIP]
> 使用 [az sql db op list](/cli/sql/db/op?#az-sql-db-op-list) 获取对数据库执行的操作列表，并使用 [az sql db op cancel](/cli/sql/db/op#az-sql-db-op-cancel) 取消对数据库的更新操作。

### <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```azurecli
az group delete --name $resource
```

## <a name="sample-reference"></a>示例参考

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| | |
|---|---|
| [az sql server](/cli/sql/server) | 服务器命令。 |
| [az sql db show-usage](/cli/sql#az-sql-show-usage) | 显示数据库的大小使用情况信息。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/)。

可以在 [Azure CLI 示例脚本](../az-cli-script-samples-content-guide.md)中找到其他 CLI 脚本示例。
