---
title: CLI 示例 - 在 Azure SQL 数据库中创建托管实例
description: 用于在 Azure SQL 数据库中创建托管实例的 Azure CLI 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 03/25/2019
ms.date: 04/27/2020
ms.openlocfilehash: ac916fd4082c58645156421cb5378290093c1c3a
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127256"
---
# <a name="use-cli-to-create-an-azure-sql-database-managed-instance"></a>使用 CLI 创建 Azure SQL 数据库托管实例

此 Azure CLI 脚本示例在新虚拟网络的专用子网中创建 Azure SQL 数据库托管实例。 它还为虚拟网络配置路由表和网络安全组。 成功运行脚本后，即可从虚拟网络或本地环境访问托管实例。 请参阅[将 Azure VM 配置为连接到 Azure SQL 数据库托管实例](../sql-database-managed-instance-configure-vm.md)和[配置从本地到 Azure SQL 数据库托管实例的点到站点连接](../sql-database-managed-instance-configure-p2s.md)。

> [!IMPORTANT]
> 有关限制，请参阅[支持的区域](../sql-database-managed-instance-resource-limits.md#supported-regions)。

本文要求运行 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>运行脚本

```azurecli
#!/bin/bash
location="China East 2"
randomIdentifier=random123

resource="resource-$randomIdentifier"
vnet="vnet-$randomIdentifier"
subnet="subnet-$randomIdentifier"
nsg="nsg-$randomIdentifier"
route="route-$randomIdentifier"
instance="instance-$randomIdentifier"

login="sampleLogin"
password="samplePassword123!"

echo "Using resource group $resource with login: $login, password: $password..."

echo "Creating $resource..."
az group create --name $resource --location "$location" 

echo "Creating $vnet with $subnet..."
az network vnet create --name $vnet --resource-group $resource --location "$location" --address-prefixes 10.0.0.0/16
az network vnet subnet create --name $subnet --resource-group $resource --vnet-name $vnet --address-prefixes 10.0.0.0/24

echo "Creating $nsg..."
az network nsg create --name $nsg --resource-group $resource --location "$location"

az network nsg rule create --name "allow_management_inbound" --nsg-name $nsg --priority 100 --resource-group $resource --access Allow --destination-address-prefixes 10.0.0.0/24 --destination-port-ranges 9000 9003 1438 1440 1452 --direction Inbound --protocol Tcp --source-address-prefixes "*" --source-port-ranges "*"
az network nsg rule create --name "allow_misubnet_inbound" --nsg-name $nsg --priority 200 --resource-group $resource --access Allow --destination-address-prefixes 10.0.0.0/24 --destination-port-ranges "*" --direction Inbound --protocol "*" --source-address-prefixes 10.0.0.0/24 --source-port-ranges "*"
az network nsg rule create --name "allow_health_probe_inbound" --nsg-name $nsg --priority 300 --resource-group $resource --access Allow --destination-address-prefixes 10.0.0.0/24 --destination-port-ranges "*" --direction Inbound --protocol "*" --source-address-prefixes AzureLoadBalancer --source-port-ranges "*"
az network nsg rule create --name "allow_management_outbound" --nsg-name $nsg --priority 1100 --resource-group $resource --access Allow --destination-address-prefixes AzureCloud --destination-port-ranges 443 12000 --direction Outbound --protocol Tcp --source-address-prefixes 10.0.0.0/24 --source-port-ranges "*"
az network nsg rule create --name "allow_misubnet_outbound" --nsg-name $nsg --priority 200 --resource-group $resource --access Allow --destination-address-prefixes 10.0.0.0/24 --destination-port-ranges "*" --direction Outbound --protocol "*" --source-address-prefixes 10.0.0.0/24 --source-port-ranges "*"

echo "Creating $route..."
az network route-table create --name $route --resource-group $resource --location "$location"

az network route-table route create --address-prefix 0.0.0.0/0 --name "primaryToMIManagementService" --next-hop-type Internet --resource-group $resource --route-table-name $route
az network route-table route create --address-prefix 10.0.0.0/24 --name "ToLocalClusterNode" --next-hop-type VnetLocal --resource-group $resource --route-table-name $route

echo "Configuring $subnet with $nsg and $route..."
az network vnet subnet update --name $subnet --network-security-group $nsg --route-table $route --vnet-name $vnet --resource-group $resource 

echo "Creating $instance with $vnet and $subnet..."
az sql mi create --admin-password $password --admin-user $login --name $instance --resource-group $resource --subnet $subnet --vnet-name $vnet --location "$location"
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
| [az network vnet](/cli/network/vnet) | 虚拟网络命令。 |
| [az network vnet subnet](/cli/network/vnet/subnet) | 虚拟网络子网命令。 |
| [az network route-table](/cli/network/route-table) | 网络路由表命令。 |
| [az sql mi](/cli/sql/mi) | 托管实例命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
