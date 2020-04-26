---
title: Azure CLI 脚本 - 缩放 Azure Database for MariaDB 服务器
description: 此示例 CLI 脚本在查询指标后将 Azure Database for MariaDB 服务器缩放为不同的性能级别。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
origin.date: 12/02/2019
ms.date: 04/13/2020
ms.openlocfilehash: 256edba30e4ffe3a9e568ba505701184090e66ef
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126918"
---
# <a name="monitor-and-scale-an-azure-database-for-mariadb-server-using-azure-cli"></a>使用 Azure CLI 监视和缩放 Azure Database for MariaDB 服务器
此示例 CLI 脚本在查询指标后为单个 Azure Database for MariaDB 服务器缩放计算和存储。 计算可以增加或减少。 存储只能增加。

本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。 

## <a name="sample-script"></a>示例脚本
使用你的订阅 ID 更新脚本。

```azurecli
#!/bin/bash

# Set up variables
$RESOURCE_GROUP="myresourcegroup"
$SERVER_NAME="mydemoserver-$RANDOM"
$PASSWORD=$(head /dev/urandom | tr -dc A-Za-z0-9 | head -c 12)
echo password $PASSWORD # returns the generated password
$LOCATION="chinaeast2"
$ADMIN_USER="myadmin"
$SUBSCRIPTION_ID="" # enter your subscription ID

# Create a resource group
az group create \
    --name $RESOURCE_GROUP \
    --location $LOCATION

# Create a MariaDB server in the resource group
az mariadb server create \
    --name $SERVER_NAME \
    --resource-group $RESOURCE_GROUP \
    --location $LOCATION \
    --admin-user $ADMIN_USER \
    --admin-password $PASSWORD \
    --sku-name GP_Gen5_2

# Monitor usage metrics - CPU
az monitor metrics list \
    --resource "/subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RESOURCE_GROUP/providers/Microsoft.DBforMariaDB/servers/$SERVER_NAME" \
    --metric cpu_percent \
    --interval PT1M

# Monitor usage metrics - Storage
az monitor metrics list \
    --resource "/subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RESOURCE_GROUP/providers/Microsoft.DBforMariaDB/servers/$SERVER_NAME" \
    --metric storage_used \
    --interval PT1M

# Scale up the server by provisionining more vCores within the same tier
az mariadb server update \
    --resource-group $RESOURCE_GROUP \
    --name $SERVER_NAME \
    --sku-name GP_Gen5_4

# Scale down the server by provisioning fewer vCores within the same tier
az mariadb server update \
    --resource-group $RESOURCE_GROUP \
    --name $SERVER_NAME \
    --sku-name GP_Gen5_2

# Scale up the server to provision a storage size of 7GB
# Storage size cannot be reduced
az mariadb server update \
    --resource-group $RESOURCE_GROUP \
    --name $SERVER_NAME \
    --storage-size 7168
```

## <a name="clean-up-deployment"></a>清理部署
运行脚本示例后，请使用以下命令删除资源组以及与其关联的所有资源。 

```azurecli
#!/bin/bash
az group delete --name myresourcegroup
```

## <a name="script-explanation"></a>脚本说明
此脚本使用下表中列出的命令：

| **命令** | **说明** |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az mariadb server create](https://docs.microsoft.com/cli/azure/mariadb/server#az-mariadb-server-create) | 创建用于托管数据库的 MariaDB 服务器。 |
| [az mariadb server update](https://docs.microsoft.com/cli/azure/mariadb/server#az-mariadb-server-update) | 更新 MariaDB 服务器的属性。 |
| [az monitor metrics list](/cli/monitor/metrics#az-monitor-metrics-list) | 列出资源的指标值。 |
| [az group delete](/cli/group#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 详细了解 [Azure Database for MariaDB 计算和存储](../concepts-pricing-tiers.md)
- 请尝试其他脚本：[Azure Database for MariaDB 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
- 详细了解 [Azure CLI](/cli/)
