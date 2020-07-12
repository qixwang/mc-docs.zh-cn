---
title: Azure CLI：在弹性池之间移动数据库
description: 使用 Azure CLI 示例脚本创建两个弹性池，并将 SQL 数据库中的数据库从一个弹性池移动到另一个弹性池。
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 06/25/2019
ms.date: 07/13/2020
ms.openlocfilehash: e3bca034d2f2ffedea80b97f3d2db18ba4a4ed8e
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86227247"
---
# <a name="use-the-azure-cli-to-move-a-database-in-sql-database-in-a-sql-elastic-pool"></a>使用 Azure CLI 移动 SQL 弹性池中 SQL 数据库中的数据库

此 Azure CLI 脚本示例创建两个弹性池，将 SQL 数据库中的池数据库从一个 SQL 弹性池移动到另一个 SQL 弹性池中，然后将池数据库从 SQL 弹性池中移出，使其成为 SQL 数据库中单独的数据库。

本主题需要运行 Azure CLI 版本 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>运行脚本

```azurecli
#!/bin/bash
location="China East 2"
randomIdentifier=random123

resource="resource-$randomIdentifier"
server="server-$randomIdentifier"
database="database-$randomIdentifier"
pool="pool-$randomIdentifier"
poolSecondary="poolsecondary-$randomIdentifier"

login="sampleLogin"
password="samplePassword123!"

echo "Creating $resource..."
az group create --name $resource --location "$location"

echo "Creating $server in $location..."
az sql server create --name $server --resource-group $resource --location "$location" --admin-user $login --admin-password $password

echo "Creating $pool and $poolSecondary..."
az sql elastic-pool create --resource-group $resource --server $server --name $pool --edition GeneralPurpose --family Gen5 --capacity 2
az sql elastic-pool create --resource-group $resource --server $server --name $poolSecondary --edition GeneralPurpose --family Gen5 --capacity 2

echo "Creating $database in $pool..."
az sql db create --resource-group $resource --server $server --name $database --elastic-pool $pool

echo "Moving $database to $poolSecondary..." # create command updates an existing datatabase
az sql db create --resource-group $resource --server $server --name $database --elastic-pool $poolSecondary

echo "Upgrade $database tier..."
az sql db create --resource-group $resource --server $server --name $database --service-objective S0
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
| [az sql server](/cli/sql/server) | 服务器命令。 |
| [az sql elastic-pools](/cli/sql/elastic-pool) | 弹性池命令。 |
| [az sql db](/cli/sql/db) | 数据库命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../az-cli-script-samples-content-guide.md)中找到。
