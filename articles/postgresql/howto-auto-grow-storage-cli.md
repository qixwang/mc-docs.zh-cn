---
title: 使用 Azure CLI 自动增长 Azure Database for PostgreSQL 存储（单一服务器）
description: 本文介绍如何使用 Azure CLI 在 Azure Database for PostgreSQL（单一服务器）中启用自动增长存储。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 8/7/2019
ms.date: 09/30/2019
ms.openlocfilehash: 9f086124374c150413b03f6d2db8678de39f688f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71309193"
---
# <a name="auto-grow-azure-database-for-postgresql-storage---single-server-using-the-azure-cli"></a>使用 Azure CLI 自动增长 Azure Database for PostgreSQL 存储（单一服务器）
本文介绍如何将 Azure Database for PostgreSQL 服务器存储配置为在不影响工作负荷的情况下增长。

[达到存储限制](/postgresql/concepts-pricing-tiers#reaching-the-storage-limit)的服务器将设置为只读。 如果启用了存储自动增长，则对于预配的存储大小小于 100 GB 的服务器，可用存储空间一旦小于 1 GB 或预配存储的 10%（以这二者中的较大值为准），预配的存储大小就会立即增加 5 GB。 对于预配的存储大小大于 100 GB 的服务器，可用存储空间小于预配的存储大小的 5% 时，预配的存储大小就会增加 5%。 [此处](/postgresql/concepts-pricing-tiers#storage)所指定的最大存储限制适用。

## <a name="prerequisites"></a>必备条件
若要完成本操作指南，需要：
- [Azure Database for PostgreSQL 服务器](quickstart-create-server-database-azure-cli.md)

> [!IMPORTANT]
> 本操作方法指南要求使用 Azure CLI 版本 2.0 或更高版本。 若要确认版本，请在 Azure CLI 命令提示符下输入 `az --version`。 若要安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="enable-postgresql-server-storage-auto-grow"></a>启用 PostgreSQL 服务器存储自动增长

使用以下命令在现有服务器上启用服务器自动增长存储：

```azurecli
az postgres server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

使用以下命令创建新服务器时启用服务器自动增长存储：

```azurecli
az postgres server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location chinaeast2 --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 9.6
```

## <a name="next-steps"></a>后续步骤

了解[如何基于指标创建警报](howto-alert-on-metric.md)。