---
title: Azure CLI：缩放弹性池
description: 使用 Azure CLI 示例脚本缩放 Azure SQL 数据库中的弹性池。
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: ''
origin.date: 06/25/2019
ms.date: 08/17/2020
ms.openlocfilehash: c6a6a376aec175f6c9701b25289166d4010f1c0c
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223117"
---
# <a name="use-the-azure-cli-to-scale-an-elastic-pool-in-azure-sql-database"></a>使用 Azure CLI 缩放 Azure SQL 数据库中的弹性池

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

此 Azure CLI 脚本示例在 Azure SQL 数据库中创建弹性池、移动共用数据库，并更改弹性池计算大小。

本主题需要运行 Azure CLI 版本 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>运行脚本

```azurecli
#!/bin/bash
location="China East"
randomIdentifier=random123

resource="resource-$randomIdentifier"
server="server-$randomIdentifier"
pool="pool-$randomIdentifier"
database="database-$randomIdentifier"
databaseAdditional="databaseadditional-$randomIdentifier"

login="sampleLogin"
password="samplePassword123!"

echo "Creating $resource..."
az group create --name $resource --location "$location"

echo "Creating $server in $location..."
az sql server create --name $server --resource-group $resource --location "$location" --admin-user $login --admin-password $password

echo "Creating $pool..."
az sql elastic-pool create --resource-group $resource --server $server --name $pool --edition GeneralPurpose --family Gen4 --capacity 5 --db-max-capacity 4 --db-min-capacity 1 --max-size 756GB

echo "Creating $database and $databaseAdditional on $server in $pool..."
az sql db create --resource-group $resource --server $server --name $database --elastic-pool $pool
az sql db create --resource-group $resource --server $server --name $databaseAdditional --elastic-pool $pool

echo "Scaling $pool..."
az sql elastic-pool update --resource-group $resource --server $server --name $pool --capacity 10 --max-size 1536GB
```

### <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```azurecli
az group delete --name $resource
```

## <a name="sample-reference"></a>示例参考

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az sql server](/cli/sql/server) | 服务器命令。 |
| [az sql db](/cli/sql/db) | 数据库命令。 |
| [az sql elastic-pools](/cli/sql/elastic-pool) | 弹性池命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../az-cli-script-samples-content-guide.md)中找到。
