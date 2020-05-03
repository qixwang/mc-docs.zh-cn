---
title: CLI 示例 - 启用 BYOK TDE - Azure SQL 数据库托管实例
description: 了解如何配置 Azure SQL 托管实例，以开始使用 BYOK 透明数据加密 (TDE) 通过 PowerShell 进行静态加密。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: azurecli
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto, carlrab
origin.date: 11/05/2019
ms.date: 04/27/2020
ms.openlocfilehash: 8215ca8ad579f6c9a5405ef57910726583e78914
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127202"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>使用 Azure Key Vault 中自己的密钥管理托管实例中的透明数据加密

此 Azure CLI 脚本示例使用 Azure Key Vault 中的密钥为 Azure SQL 托管实例配置使用客户托管密钥的透明数据加密 (TDE)。 这通常称为 TDE 的自带密钥方案。 若要详细了解使用客户托管密钥的 TDE，请参阅[适用于 Azure SQL 的支持“创建自己的密钥”的 TDE](../transparent-data-encryption-byok-azure-sql.md)。

本文要求运行 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

### <a name="prerequisites"></a>先决条件

现有托管实例，请参阅[使用 Azure CLI 创建 Azure SQL 数据库托管实例](sql-database-create-configure-managed-instance-cli.md)。

### <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>运行脚本

```azurecli
#!/bin/bash
instance="<instanceId>" # add instance here
resource="<resourceId>" # add resource here

location="China East 2"
randomIdentifier=random123

vault="vault-$randomIdentifier"
key="key-$randomIdentifier"

echo "Creating $vault..."
az keyvault create --name $vault --resource-group $resource --enable-soft-delete true --location "$location"

echo "Setting policy on $vault..."
$instanceId = az sql mi show --name $instance --resource-group $resource -o json --query identity.principalId

az keyvault set-policy --name $vault --key-permissions get, unwrapKey, wrapKey --object-id $instanceId

echo "Creating $key..."
az keyvault key create --name $key --vault-name $vault --size 2048 

#keyPath="C:\yourFolder\yourCert.pfx"
#keyPassword="yourPassword" 
#az keyvault certificate import --file $keyPath --name $key --vault-name $vault --password $keyPassword

echo "Setting security on $instance with $key..."
keyId=$(az keyvault key show --name $key --vault-name $vault -o json --query key.kid | tr -d '"')

az sql mi key create --kid $keyId --managed-instance $instance --resource-group $resource
az sql mi tde-key set --server-key-type AzureKeyVault --kid $keyId --managed-instance $instance --resource-group $resource
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
| [az sql db](/cli/sql/db) | 数据库命令。 |
| [az sql failover-group](/cli/sql/failover-group) | 故障转移组命令。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
