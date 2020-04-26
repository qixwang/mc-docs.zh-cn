---
title: Azure CLI 脚本 - 创建 Azure Database for MariaDB
description: 此示例 CLI 脚本创建 Azure Database for MariaDB 服务器，并配置服务器级防火墙规则。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
origin.date: 11/28/2018
ms.date: 04/13/2020
ms.openlocfilehash: 0136a288bd2d902cb72841a49e26b22b7dc39a49
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126791"
---
# <a name="create-a-mariadb-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>使用 Azure CLI 创建 MariaDB 服务器并配置防火墙规则
此示例 CLI 脚本创建 Azure Database for MariaDB 服务器，并配置服务器级防火墙规则。 成功运行此脚本后，可以通过所有 Azure 服务和配置的 IP 地址访问 MariaDB 服务器。

本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。

```azurecli
#!/bin/bash

# Create a resource group
az group create \
--name myresourcegroup \
--location chinaeast2

# Create a MariaDB server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure.
# Substitute the <server_admin_password> with your own value.
az mariadb server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location chinaeast2 \
--admin-user myadmin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2 \

# Configure a firewall rule for the server
# The ip address range that you want to allow to access your server
az mariadb server firewall-rule create \
--resource-group myresourcegroup \
--server mydemoserver \
--name AllowIps \
--start-ip-address 0.0.0.0 \
--end-ip-address 255.255.255.255
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
| [az mariadb server firewall create](https://docs.microsoft.com/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create) | 创建一个防火墙规则，以允许从输入的 IP 地址范围访问服务器及其下的所有数据库。 |
| [az group delete](/cli/group#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。
- 尝试其他脚本：[Azure Database for MariaDB 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
