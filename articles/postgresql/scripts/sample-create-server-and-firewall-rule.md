---
title: Azure CLI 脚本 - 创建 Azure Database for PostgreSQL
description: Azure CLI 示例脚本 - 为 PostgreSQL 服务器创建 Azure 数据库，并配置服务器级防火墙规则。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
origin.date: 02/28/2018
ms.date: 02/18/2019
ms.openlocfilehash: 4a58e1d738939f6b7b995e40b6f535414640179b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63856975"
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>使用 Azure CLI 为 PostgreSQL 服务器创建 Azure 数据库并配置防火墙规则
此示例 CLI 脚本为 PostgreSQL 服务器创建 Azure 数据库，并配置服务器级防火墙规则。 成功运行此脚本后，可以通过所有 Azure 服务和配置的 IP 地址访问 PostgreSQL 服务器。

本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。

```cli
#!/bin/bash

# Create a resource group
az group create \
--name myresourcegroup \
--location chinaeast2

# Create a PostgreSQL server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure.
# Substitute the <server_admin_password> with your own value.
az postgres server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location chinaeast2 \
--admin-user myadmin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2 \

# Configure a firewall rule for the server
# The ip address range that you want to allow to access your server
az postgres server firewall-rule create \
--resource-group myresourcegroup \
--server mydemoserver \
--name AllowIps \
--start-ip-address 0.0.0.0 \
--end-ip-address 255.255.255.255
```

## <a name="clean-up-deployment"></a>清理部署
运行脚本示例后，请使用以下命令删除资源组以及与其关联的所有资源。 

```bash
#!/bin/bash
az group delete --name myresourcegroup
```

## <a name="script-explanation"></a>脚本说明
此脚本使用下表中列出的命令：

| **命令** | **说明** |
|---|---|
| [az group create](/cli/group) | 创建用于存储所有资源的资源组。 |
| [az postgres server create](/cli/postgres/server) | 创建托管数据库的 PostgreSQL 服务器。 |
| [az postgres server firewall create](/cli/postgres/server/firewall-rule) | 创建一个防火墙规则，以允许从输入的 IP 地址范围访问服务器及其下的所有数据库。 |
| [az group delete](/cli/group) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)
- 尝试其他脚本：[Azure Database for PostgreSQL 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
