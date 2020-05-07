---
title: 使用客户管理的密钥进行静态加密
description: 了解 Azure 容器注册表的静态加密，以及如何使用 Azure Key Vault 中存储的客户管理的密钥来加密注册表
ms.topic: article
origin.date: 03/10/2020
ms.date: 04/06/2020
ms.author: v-yeche
ms.custom: ''
ms.openlocfilehash: aa250dc6a87d96cae43cd01822a5e259835e99ff
ms.sourcegitcommit: 564739de7e63e19a172122856ebf1f2f7fb4bd2e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82093538"
---
# <a name="encryption-using-customer-managed-keys"></a>使用客户管理的密钥进行加密

当你在 Azure 容器注册表中存储映像和其他项目时，Azure 会自动使用服务托管的密钥对注册表内容进行静态加密。 可以使用在 Azure Key Vault 中创建和管理的密钥，通过一个附加的加密层来补充默认加密。 本文将引导你使用 Azure CLI 和 Azure 门户完成这些步骤。

<!--Not Available on [service-managed keys](../security/fundamentals/encryption-atrest.md#data-encryption-models)-->

使用客户管理的密钥进行服务器端加密，是通过与 [Azure Key Vault](../key-vault/key-vault-overview.md) 的集成来受到支持的。 你可以创建自己的加密密钥并将其存储在 Key Vault 中，或使用 Azure Key Vault 的 API 来生成加密密钥。 使用 Azure Key Vault 还可以审核密钥用法。

此功能在“高级”容器注册表服务层级中可用。  有关注册表服务层级和限制的信息，请参阅 [Azure 容器注册表 SKU](container-registry-skus.md)。

> [!IMPORTANT]
> 此功能目前以预览版提供，存在一些[限制](#preview-limitations)。 需同意[补充使用条款][terms-of-use]才可使用预览版。 在正式版 (GA) 推出之前，此功能的某些方面可能会有所更改。
>

## <a name="preview-limitations"></a>预览版限制 

* 目前只能在创建注册表时启用此功能。
* 对注册表启用客户管理的密钥后，无法禁用此功能。
* 使用客户管理的密钥加密的注册表目前不支持[内容信任](container-registry-content-trust.md)。
* 在使用客户管理的密钥加密的注册表中，[ACR 任务](container-registry-tasks-overview.md)的运行日志目前只会保留 24 小时。 如果需要将日志保留更长时间，请参阅有关[导出和存储任务运行日志](container-registry-tasks-logs.md#alternative-log-storage)的指南。

## <a name="prerequisites"></a>先决条件

若要使用本文中所述的 Azure CLI 步骤，需要安装 Azure CLI 2.2.0 或更高版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="enable-customer-managed-key---cli"></a>启用客户管理的密钥 - CLI

### <a name="create-a-resource-group"></a>创建资源组

如果需要，请运行 [az group create][az-group-create] 命令创建一个资源组以用于创建密钥保管库、容器注册表和其他所需资源。

```azurecli
az group create --name <resource-group-name> --location <location>
```

### <a name="create-a-user-assigned-managed-identity"></a>创建用户分配的托管标识

使用 [az identity create][az-identity-create] 命令为 Azure 资源创建用户分配的[托管标识](../active-directory/managed-identities-azure-resources/overview.md)。 注册表将使用此标识来访问 Key Vault 服务。

```azurecli
az identity create \
  --resource-group <resource-group-name> \
  --name <managed-identity-name> 
```

记下命令输出中的以下值：`id` 和 `principalId`。 在后续步骤中，需要使用这些值来配置注册表对密钥保管库的访问。

```JSON
{
  "clientId": "xxxx2bac-xxxx-xxxx-xxxx-192cxxxx6273",
  "clientSecretUrl": "https://control-chinanorth.identity.chinacloudapi.cn/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myidentityname/credentials?tid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&oid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&aid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myresourcegroup",
  "location": "chinanorth",
  "name": "myidentityname",
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

为方便起见，请将这些值存储在环境变量中：

```azurecli
identityID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'id' --output tsv)

identityPrincipalID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'principalId' --output tsv)
```

### <a name="create-a-key-vault"></a>创建密钥保管库

使用 [az keyvault create][az-keyvault-create] 创建一个密钥保管库来存储客户管理的密钥，以便进行注册表加密。 

为了防止意外删除密钥或密钥保管库而导致数据丢失，必须启用以下设置：“软删除”和“清除保护”。   以下示例包含这些设置的参数： 

```azurecli
az keyvault create --name <key-vault-name> \
  --resource-group <resource-group-name> \
  --enable-soft-delete \
  --enable-purge-protection
```

### <a name="add-key-vault-access-policy"></a>添加密钥保管库访问策略

配置针对密钥保管库的策略，使标识可以访问密钥保管库。 在以下 [az keyvault set-policy][az-keyvault-set-policy] 命令中，请传递已创建的托管标识的主体 ID（以前存储在环境变量中）。 将密钥权限设置为 get、unwrapKey 和 wrapKey。     

```azurecli
az keyvault set-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID \
  --key-permissions get unwrapKey wrapKey 
```

### <a name="create-key-and-get-key-id"></a>创建密钥并获取密钥 ID

运行 [az keyvault key create][az-keyvault-key-create] 命令以在密钥保管库中创建密钥。

```azurecli
az keyvault key create \
  --name <key-name> \
  --vault-name <key-vault-name>
```

记下命令输出中的密钥 ID `kid`。 在下一步骤中将会用到此 ID：

```JSON
[...]
  "key": {
    "crv": null,
    "d": null,
    "dp": null,
    "dq": null,
    "e": "AQAB",
    "k": null,
    "keyOps": [
      "encrypt",
      "decrypt",
      "sign",
      "verify",
      "wrapKey",
      "unwrapKey"
    ],
    "kid": "https://mykeyvault.vault.azure.cn/keys/mykey/xxxxxxxxxxxxxxxxxxxxxxxx",
    "kty": "RSA",
[...]
```
为方便起见，请将此值存储在环境变量中：

```azurecli
keyID=$(az keyvault key show --name <keyname> --vault-name <key-vault-name> --query 'key.kid' --output tsv)
```

### <a name="create-a-registry-with-customer-managed-key"></a>使用客户管理的密钥创建注册表

运行 [az acr create][az-acr-create] 命令，以创建注册表并启用客户管理的密钥。 传递托管标识主体 ID 和密钥 ID（以前已存储在环境变量中）：

```azurecli
az acr create \
  --resource-group <resource-group-name> \
  --name <container-registry-name> \
  --identity $identityID \
  --key-encryption-key $keyID \
  --sku Premium
```

### <a name="show-encryption-status"></a>显示加密状态

若要显示是否启用了使用客户管理的密钥进行注册表加密，请运行 [az acr encryption show][az-acr-encryption-show] 命令：

```azurecli
az acr encryption show --name <registry-name> 
```

## <a name="enable-customer-managed-key---portal"></a>启用客户管理的密钥 - 门户

### <a name="create-a-managed-identity"></a>创建托管标识

在 Azure 门户中为 Azure 资源创建用户分配的[托管标识](../active-directory/managed-identities-azure-resources/overview.md)。 有关步骤，请参阅[创建用户分配的标识](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)。

记下该托管标识的“资源名称”。  在后续步骤中需要用到此名称。

![在 Azure 门户中创建用户分配的托管标识](./media/container-registry-customer-managed-keys/create-managed-identity.png)

### <a name="create-a-key-vault"></a>创建密钥保管库

有关创建密钥保管库的步骤，请参阅[快速入门：使用 Azure 门户在 Azure Key Vault 中设置和检索机密](../key-vault/quick-create-portal.md)。

为客户管理的密钥创建密钥保管库时，必须在“基本信息”选项卡中启用以下保护设置：  “软删除”和“清除保护”。   这些设置可以帮助防止意外删除密钥或密钥保管库而导致的数据丢失。

![在 Azure 门户中创建密钥保管库](./media/container-registry-customer-managed-keys/create-key-vault.png)

### <a name="add-key-vault-access-policy"></a>添加密钥保管库访问策略

配置针对密钥保管库的策略，使标识可以访问密钥保管库。

1. 导航到你的密钥保管库。
1. 选择“设置” > “访问策略”>“+添加访问策略”。  
1. 选择“密钥权限”，然后选择“获取”、“解包密钥”和“包装密钥”。    
1. 选择“选择主体”，然后选择用户分配的托管标识的资源名称。   
1. 依次选择“添加”、“保存”。  

![创建密钥保管库访问策略](./media/container-registry-customer-managed-keys/add-key-vault-access-policy.png)

### <a name="create-key"></a>创建密钥

1. 导航到你的密钥保管库。
1. 选择“设置”   > “密钥”  。
1. 选择“+生成/导入”并输入密钥的唯一名称。 
1. 接受剩余的默认值，然后选择“创建”。 
1. 创建后，选择该密钥并记下当前密钥版本。

### <a name="create-azure-container-registry"></a>创建 Azure 容器注册表
<!--MOONCAKE: CUSTOMIZE-->

1. 选择“创建资源”，在“新建”页的搜索筛选器中键入“容器注册表”，然后按回车键。   
1. 在搜索结果中选择“容器注册表”对应的项，然后选择“创建”。  
    
    <!--MOONCAKE: CUSTOMIZE-->
    
1. 在“基本信息”选项卡中选择或创建一个资源组，然后输入注册表名称。  在“SKU”中选择“高级”。  
1. 在“加密”选项卡上的“客户管理的密钥”中，选择“已启用”。   
1. 在“标识”中，选择你创建的托管标识。 
1. 在“加密密钥”中，选择“从 Key Vault 选择”。  
1. 在“从 Azure Key Vault 选择密钥”窗口中，选择在上一部分创建的密钥保管库、密钥和版本。 
1. 在“加密”选项卡中，选择“查看 + 创建”。  
1. 选择“创建”以部署注册表实例。 

![在 Azure 门户中创建容器注册表](./media/container-registry-customer-managed-keys/create-encrypted-registry.png)

若要在门户中查看注册表的加密状态，请导航到注册表。 在“设置”下，选择“加密(预览版)”。  

## <a name="enable-customer-managed-key---template"></a>启用客户管理的密钥 - 模板

还可以使用资源管理器模板来创建注册表，并启用使用客户管理的密钥进行加密。 

以下模板创建新的容器注册表和用户分配的托管标识。 将以下内容复制到新文件，并使用 `CMKtemplate.json` 等文件名保存该文件。

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vault_name": {
      "defaultValue": "",
      "type": "String"
    },
    "registry_name": {
      "defaultValue": "",
      "type": "String"
    },
    "identity_name": {
      "defaultValue": "",
      "type": "String"
    },
    "kek_id": {
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.ContainerRegistry/registries",
      "apiVersion": "2019-12-01-preview",
      "name": "[parameters('registry_name')]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Premium",
        "tier": "Premium"
      },
      "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
          "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]": {}
        }
      },
      "dependsOn": [
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]"
      ],
      "properties": {
        "adminUserEnabled": false,
        "encryption": {
          "status": "enabled",
          "keyVaultProperties": {
            "identity": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name')), '2018-11-30').clientId]",
            "KeyIdentifier": "[parameters('kek_id')]"
          }
        },
        "networkRuleSet": {
          "defaultAction": "Allow",
          "virtualNetworkRules": [],
          "ipRules": []
        },
        "policies": {
          "quarantinePolicy": {
            "status": "disabled"
          },
          "trustPolicy": {
            "type": "Notary",
            "status": "disabled"
          },
          "retentionPolicy": {
            "days": 7,
            "status": "disabled"
          }
        }
      }
    },
    {
      "type": "Microsoft.KeyVault/vaults/accessPolicies",
      "apiVersion": "2018-02-14",
      "name": "[concat(parameters('vault_name'), '/add')]",
      "dependsOn": [
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]"
      ],
      "properties": {
        "accessPolicies": [
          {
            "tenantId": "[subscription().tenantId]",
            "objectId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name')), '2018-11-30').principalId]",
            "permissions": {
              "keys": [
                "get",
                "unwrapKey",
                "wrapKey"
              ]
            }
          }
        ]
      }
    },
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "apiVersion": "2018-11-30",
      "name": "[parameters('identity_name')]",
      "location": "[resourceGroup().location]"
    }
  ]
}

```

按照前面部分中的步骤创建以下资源：

* 密钥保管库，按名称标识
* 密钥保管库密钥，按密钥 ID 标识

运行以下 [az group deployment create][az-group-deployment-create] 命令，以使用前面的模板文件创建注册表。 根据指示提供新的注册表名称和托管标识名称，以及你创建的密钥保管库名称和密钥 ID。 

```bash
az group deployment create \
  --resource-group <resource-group-name> \
  --template-file CMKtemplate.json \
  --parameters \
    registry_name=<registry-name> \
    identity_name=<managed-identity> \
    vault_name=<key-vault-name> \
    kek_id=<key-vault-key-id>
```

### <a name="show-encryption-status"></a>显示加密状态

若要显示注册表加密的状态，请运行 [az acr encryption show-status][az-acr-encryption-show-status] 命令：

```azurecli
az acr encryption show-status --name <registry-name> 
```

## <a name="use-the-registry"></a>使用注册表

在启用注册表以使用客户管理的密钥加密数据后，可以像在未使用客户管理的密钥加密的注册表中执行操作一样，执行相同的注册表操作。 例如，可向注册表进行身份验证和推送 Docker 映像。 请参阅[推送和提取映像](container-registry-get-started-docker-cli.md)中的示例命令。

## <a name="rotate-key"></a>轮换密钥

可以根据自己的合规性策略，在 Azure Key Vault 中轮换客户管理的密钥。 创建新密钥，然后更新注册表以使用新密钥加密数据。 可以使用 Azure CLI 或者在门户中执行这些步骤。

例如，运行 [az keyvault key create][az-keyvault-key-create] 命令来创建新密钥：

```azurecli
az keyvault key create --name <new-key-name> --vault-name <key-vault-name> 
```

然后运行 [az acr encryption rotate-key][az-acr-encryption-rotate-key] 命令，并传递前面配置的托管标识的新密钥 ID 和主体 ID：

```azurecli
az acr encryption rotatekey \
  --name <registry-name> \
  --key-encryption-key <new-key-id> \
  --identity $identityPrincipalID
```

## <a name="revoke-key"></a>撤销密钥

通过更改针对密钥保管库的访问策略或者通过删除密钥，来撤销客户管理的加密密钥。 例如，使用 [az keyvault delete-policy][az-keyvault-delete-policy] 命令更改注册表使用的托管标识的访问策略。 例如：

```azurecli
az keyvault delete-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID
```

由于注册表无法访问加密密钥，撤销密钥会有效阻止对所有注册表数据的访问。 如果启用了对密钥的访问或者还原了已删除的密钥，则注册表将选取该密钥，使你可以再次访问已加密的注册表数据。

## <a name="next-steps"></a>后续步骤

<!--Not Available on [encryption at rest in Azure](../security/fundamentals/encryption-atrest.md)-->

* 详细了解访问策略以及如何[保护对密钥保管库的访问](../key-vault/key-vault-secure-your-key-vault.md)。
* 若要针对 Azure 容器注册表的客户管理的密钥提供反馈，请访问 [ACR GitHub 站点](https://aka.ms/acr/issues)。

<!-- LINKS - external -->

[terms-of-use]: https://www.azure.cn/support/legal/subscription-agreement

<!-- LINKS - internal -->

[az-feature-register]: https://docs.azure.cn/cli/feature?view=azure-cli-latest#az-feature-register
[az-feature-show]: https://docs.azure.cn/cli/feature?view=azure-cli-latest#az-feature-show
[az-group-create]: https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create
[az-identity-create]: https://docs.microsoft.com/cli/azure/identity?view=azure-cli-latest#az-identity-create
[az-feature-register]: https://docs.azure.cn/cli/feature?view=azure-cli-latest#az-feature-register
[az-group-deployment-create]: https://docs.azure.cn/cli/group/deployment?view=azure-cli-latest#az-group-deployment-create
[az-keyvault-create]: https://docs.azure.cn/cli/keyvault?view=azure-cli-latest#az-keyvault-create
[az-keyvault-key-create]: https://docs.azure.cn/cli/keyvault/key?view=azure-cli-latest#az-keyvault-key-create
[az-keyvault-set-policy]: https://docs.azure.cn/cli/keyvault?view=azure-cli-latest#az-keyvault-set-policy
[az-keyvault-delete-policy]: https://docs.azure.cn/cli/keyvault?view=azure-cli-latest#az-keyvault-delete-policy
[az-resource-show]: https://docs.azure.cn/cli/resource?view=azure-cli-latest#az-resource-show
[az-acr-create]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-create
[az-acr-show]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-show
[az-acr-encryption-rotate-key]: https://docs.azure.cn/cli/acr/encryption?view=azure-cli-latest#az-acr-encryption-rotate-key
[az-acr-encryption-show]: https://docs.azure.cn/cli/acr/encryption?view=azure-cli-latest#az-acr-encryption-show

<!-- Update_Description: new article about container registry customer managed keys -->
<!--NEW.date: 04/06/2020-->