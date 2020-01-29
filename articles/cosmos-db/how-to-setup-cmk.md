---
title: 为 Azure Cosmos DB 帐户配置客户管理的密钥
description: 了解如何为 Azure Cosmos DB 帐户配置客户管理的密钥
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 01/11/2020
ms.date: 01/20/2020
ms.author: v-yeche
ROBOTS: noindex, nofollow
ms.openlocfilehash: f4b9c4ba6f1a1de05310fbc26249c1020fa8e050
ms.sourcegitcommit: a2e13b4f20aa85e5d696fa3233be14e522e76ba6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288540"
---
# <a name="configure-customer-managed-keys-for-your-azure-cosmos-db-account"></a>为 Azure Cosmos DB 帐户配置客户管理的密钥

> [!NOTE]
> 目前，必须请求访问权限才能使用此功能。 为此，请联系 [cosmosdbpm@microsoft.com](mailto:cosmosdbpm@microsoft.com)。

存储在 Azure Cosmos DB 帐户中的数据会自动无缝地进行加密。 Azure Cosmos DB 提供两个选项让你管理用于加密静态数据的密钥：
- **服务托管的密钥**。 默认情况下，Azure 会管理用于加密 Azure Cosmos DB 帐户的密钥。
- **客户管理的密钥 (CMK)** 。 可以根据需要使用你自己管理的密钥来添加另一个加密层。

客户管理的密钥必须存储在 [Azure Key Vault](../key-vault/key-vault-overview.md) 中。 必须为每个启用了 CMK 的帐户提供一个密钥，该密钥用于加密该帐户中存储的所有数据。

## <a name="setup"></a>设置

目前，客户管理的密钥仅可用于新帐户，并需要在创建帐户期间进行设置。

### <a name="1-make-sure-the-azure-cosmos-db-resource-provider-is-registered-for-your-azure-subscription"></a>1.确保为 Azure 订阅注册了 Azure Cosmos DB 资源提供程序

在 Azure 门户中转到你的 Azure 订阅，然后在左侧菜单中选择“资源提供程序”： 

![左侧菜单中的“资源提供程序”项](./media/how-to-setup-cmk/portal-rp.png)

搜索 **Microsoft.DocumentDB** 资源提供程序。
- 如果该资源提供程序标记为已注册，则无需执行任何操作。
- 否则，请选择它并单击“注册”： 

    ![注册 Microsoft.DocumentDB 资源提供程序](./media/how-to-setup-cmk/portal-rp-register.png)

### <a name="2-configure-your-azure-key-vault-instance"></a>2.配置 Azure Key Vault 实例

对 Azure Cosmos DB 使用客户管理的密钥需要在你打算用来托管加密密钥的 Azure Key Vault 实例上设置两个属性：“软删除”和“不清除”。   这些属性默认未启用，但可以使用 PowerShell 或 Azure CLI 来启用。

若要了解如何在现有 Azure Key Vault 实例上启用这些属性，请参阅以下文章之一中标题为“启用软删除”和“启用清除保护”的部分：
- [如何通过 PowerShell 使用软删除](../key-vault/key-vault-soft-delete-powershell.md)
- [如何通过 Azure CLI 使用软删除](../key-vault/key-vault-soft-delete-cli.md)

### <a name="3-add-an-access-policy-to-your-azure-key-vault-instance"></a>3.将访问策略添加到 Azure Key Vault 实例

在 Azure 门户中，转到你打算用来托管加密密钥的 Azure Key Vault 实例。 然后在左侧菜单中选择“访问策略”： 

![左侧菜单中的“访问策略”](./media/how-to-setup-cmk/portal-akv-ap.png)

- 选择“+ 添加访问策略” 
- 在“密钥权限”下拉菜单中，选择“获取”、“解包密钥”和“包装密钥”：    

    ![选择适当的权限](./media/how-to-setup-cmk/portal-akv-add-ap-perm2.png)

- 在“选择主体”下，选择“未选择任何项”。   然后搜索并选择“Azure Cosmos DB”主体。  最后，单击底部的“选择”（如果找不到“Azure Cosmos DB”主体，可能需要根据步骤 1 所述重新注册 **Microsoft.DocumentDB** 资源提供程序）：  

    ![选择 Azure Cosmos DB 主体](./media/how-to-setup-cmk/portal-akv-add-ap.png)

- 选择“添加”以添加新的访问策略 

### <a name="4-generate-a-key-in-azure-key-vault"></a>4.在 Azure Key Vault 中生成密钥

在 Azure 门户中，转到你打算用来托管加密密钥的 Azure Key Vault 实例。 然后在左侧菜单中选择“密钥”： 

![左侧菜单中的“密钥”项](./media/how-to-setup-cmk/portal-akv-keys.png)

- 选择“生成/导入” 
- 提供新密钥的名称，选择 RSA 密钥大小（为获得最高安全性，建议最少选择 3072），然后选择“创建”： 

    ![创建新密钥](./media/how-to-setup-cmk/portal-akv-gen.png)

- 创建密钥后，单击新建的密钥，然后单击其当前版本
- 复制密钥的“密钥标识符”（但不要复制最后一个正斜杠后面的部分）： 

    ![复制密钥的密钥标识符](./media/how-to-setup-cmk/portal-akv-keyid.png)

### <a name="5-create-a-new-azure-cosmos-db-account"></a>5.创建新的 Azure Cosmos DB 帐户

<!--Not Available on #### Using the Azure portal-->

#### <a name="using-powershell"></a>使用 PowerShell

使用 PowerShell 创建新的 Azure Cosmos DB 帐户时，
- 请在 **PropertyObject** 中的 **keyVaultKeyUri** 属性下，传递步骤 4 中复制的 Azure Key Vault 密钥 URI，
- 确保使用“2019-12-12”作为 API 版本。

> [!IMPORTANT]
> 必须显式设置 `Location` 参数，才能使用 CMK 成功创建帐户。

```powershell
$resourceGroupName = "myResourceGroup"
$accountLocation = "China North 2"
$accountName = "mycosmosaccount"

$failoverLocations = @(
    @{ "locationName"="China North 2"; "failoverPriority"=0 }
)

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$failoverLocations;
    "keyVaultKeyUri" = "https://<my-vault>.vault.azure.cn/keys/<my-key>";
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2019-12-12" -ResourceGroupName $resourceGroupName `
    -Location $accountLocation -Name $accountName -PropertyObject $CosmosDBProperties
```

#### <a name="using-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板

通过 Azure 资源管理器模板创建新的 Azure Cosmos DB 帐户时：
- 在 **properties** 对象中的 **keyVaultKeyUri** 属性下，传递步骤 4 中复制的 Azure Key Vault 密钥 URI
- 确保使用“2019-12-12”作为 API 版本

> [!IMPORTANT]
> 必须显式设置 `location` 参数，才能使用 CMK 成功创建帐户。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "keyVaultKeyUri": {
            "type": "string"
        }
    },
    "resources": 
    [
        {
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "name": "[parameters('accountName')]",
            "apiVersion": "2019-12-12",
            "kind": "GlobalDocumentDB",
            "location": "[parameters('location')]",
            "properties": {
                "locations": [ 
                    {
                        "locationName": "[parameters('location')]",
                        "failoverPriority": 0,
                        "isZoneRedundant": false
                    }
                ],
                "databaseAccountOfferType": "Standard",
                "keyVaultKeyUri": "[parameters('keyVaultKeyUri')]"
            }
        }
    ]
}

```

```powershell
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$accountLocation = "China North 2"
$keyVaultKeyUri = "https://<my-vault>.vault.azure.cn/keys/<my-key>"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "deploy.json" `
    -accountName $accountName `
    -location $accountLocation `
    -keyVaultKeyUri $keyVaultKeyUri
```

## <a name="frequently-asked-questions"></a>常见问题

### <a name="is-there-any-additional-charge-when-using-customer-managed-keys"></a>使用客户管理的密钥是否需要支付额外的费用？

是的。 由于使用客户管理的密钥管理数据加密和解密需要增大计算负载，针对 Azure Cosmos DB 帐户执行的所有操作消耗的[请求单位](./request-units.md)数会增加 25%。

### <a name="what-data-gets-encrypted-with-the-cmk"></a>哪些数据是通过 CMK 加密的？

Azure Cosmos DB 帐户中存储的所有数据都将通过 CMK 加密，但以下元数据除外：
- Azure Cosmos DB [帐户、数据库和容器](./account-overview.md#elements-in-an-azure-cosmos-account)的名称；
- [存储过程](./stored-procedures-triggers-udfs.md)的名称；
- [索引策略](./index-policy.md)中声明的属性路径；
- 容器[分区键](./partitioning-overview.md)的值。

### <a name="will-customer-managed-keys-be-supported-for-existing-accounts"></a>现有的帐户是否支持客户管理的密钥？

此功能目前仅适用于新帐户。

### <a name="is-there-a-plan-to-support-finer-granularity-than-account-level-keys"></a>你们是否有计划支持比帐户级密钥更精细的粒度？

目前无此计划，但我们正在考虑容器级密钥。

### <a name="how-does-customer-managed-keys-affect-backups"></a>客户管理的密钥对备份造成怎样的影响？

Azure Cosmos DB [定期自动备份](./online-backup-and-restore.md)帐户中存储的数据。 此操作会备份已加密的数据。 要使还原的备份可用，备份时使用的加密密钥仍必须可用。 这意味着，不应进行任何吊销操作，并且备份时使用的密钥版本仍应保持启用状态。

### <a name="how-do-i-revoke-an-encryption-key"></a>如何吊销加密密钥？

可以通过禁用密钥的最新版本来吊销密钥：

![禁用密钥的版本](./media/how-to-setup-cmk/portal-akv-rev2.png)

或者，若要从 Azure Key Vault 实例中吊销所有密钥，可以删除授予 Azure Cosmos DB 主体的访问策略：

![删除 Azure Cosmos DB 主体的访问策略](./media/how-to-setup-cmk/portal-akv-rev.png)

### <a name="what-operations-are-available-after-a-customer-managed-key-is-revoked"></a>吊销客户管理的密钥后可执行哪些操作？

吊销加密密钥后，删除帐户是唯一可执行的操作。

## <a name="next-steps"></a>后续步骤

- 详细了解 [Azure Cosmos DB 中的数据加密](./database-encryption-at-rest.md)
- 获取[安全访问 Cosmos DB 中的数据](secure-access-to-data.md)的概述

<!-- Update_Description: new article about how to setup cmk -->
<!--NEW.date: 01/20/2020-->