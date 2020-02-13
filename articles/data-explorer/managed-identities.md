---
title: 如何配置 Azure 数据资源管理器群集的托管标识
description: 了解如何配置 Azure 数据资源管理器群集的托管标识。
author: saguiitay
ms.author: v-tawe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
origin.date: 01/06/2020
ms.date: 02/17/2020
ms.openlocfilehash: 038006cc99063c0ae94bdb4827443a19a1765d5f
ms.sourcegitcommit: 7c80405a6b48380814b4b414e9f8a5756c007880
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067779"
---
# <a name="configure-managed-identities-for-your-azure-data-explorer-cluster"></a>配置 Azure 数据资源管理器群集的托管标识

借助 [Azure Active Directory 中的托管标识](/active-directory/managed-identities-azure-resources/overview)，群集可以轻松访问其他受 AAD 保护的资源（例如 Azure Key Vault）。 标识由 Azure 平台托管，无需预配或轮换任何机密。 本文介绍如何为 Azure 数据资源管理器群集创建托管标识。 

> [!Note]
> 如果在订阅或租户之间迁移了应用，Azure 数据资源管理器的托管标识将不按预期方式运行。 应用将需要获取新标识，为此，可以使用[删除标识](#remove-an-identity)来禁用并重新启用该功能。 还需要更新下游资源的访问策略才能使用新标识。

## <a name="add-a-system-assigned-identity"></a>添加系统分配的标识

可为群集分配一个绑定到群集的、并在删除群集时被删除的**系统分配的标识**。 一个群集只能有一个系统分配的标识。 使用系统分配的标识创建群集需要在该群集上设置一个额外的属性。

### <a name="add-a-system-assigned-identity-using-c"></a>使用 C# 添加系统分配的标识

若要使用 Azure 数据资源管理器 C# 客户端设置托管标识，请执行以下操作：

* 安装 [Azure 数据资源管理器 (Kusto) NuGet 包](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)。
* 安装 [Microsoft.IdentityModel.Clients.ActiveDirectory NuGet 包](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)以进行身份验证。
* 若要运行以下示例，请[创建可以访问资源的 Azure AD 应用程序](/active-directory/develop/howto-create-service-principal-portal)和服务主体。 可以在订阅范围添加角色分配，并获取所需的 `Directory (tenant) ID`、`Application ID` 和 `Client Secret`。

#### <a name="create-or-update-your-cluster"></a>创建或更新群集

1. 使用 `Identity` 属性创建或更新群集：

    ```csharp
    var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
    var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
    var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
    var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
    var authenticationContext = new AuthenticationContext($"https://login.partner.microsoftonline.cn/{tenantId}");
    var credential = new ClientCredential(clientId, clientSecret);
    var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.chinacloudapi.cn/", clientCredential: credential);
    
    var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);
    
    var kustoManagementClient = new KustoManagementClient(credentials)
    {
        SubscriptionId = subscriptionId
    };
    
    var resourceGroupName = "testrg";
    var clusterName = "mykustocluster";
    var location = "China East 2";
    var skuName = "Standard_D13_v2";
    var tier = "Standard";
    var capacity = 5;
    var sku = new AzureSku(skuName, tier, capacity);
    var identity = new Identity(IdentityType.SystemAssigned);
    var cluster = new Cluster(location, sku, identity: identity);
    await kustoManagementClient.Clusters.CreateOrUpdateAsync(resourceGroupName, clusterName, cluster);
    ```
    
2. 运行以下命令，检查是否已使用标识成功创建或更新群集：

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

    如果结果中包含具有 `Succeeded` 值的 `ProvisioningState`，则表示已创建或更新群集，群集应具有以下属性：
   
    ```csharp
    var principalId = cluster.Identity.PrincipalId;
    var tenantId = cluster.Identity.TenantId;
    ```

    `PrincipalId` 和 `TenantId` 已替换为 GUID。 `TenantId` 属性标识该标识所属的 AAD 租户。 `PrincipalId` 是群集的新标识的唯一标识符。 在 AAD 中，服务主体的名称与你为应用服务或 Azure Functions 实例提供的名称相同。

### <a name="add-a-system-assigned-identity-using-an-azure-resource-manager-template"></a>使用 Azure 资源管理器模板添加系统分配的标识

Azure 资源管理器模板可以用于自动化 Azure 资源部署。 若要详细了解如何部署到 Azure 数据资源管理器，请参阅[使用 Azure 资源管理器模板创建 Azure 数据资源管理器群集和数据库](create-cluster-database-resource-manager.md)。

添加系统分配的类型将告知 Azure 要为群集创建和管理标识。 在资源定义包括以下属性，可以创建 `Microsoft.Kusto/clusters` 类型的任何有标识资源： 

```json
"identity": {
    "type": "SystemAssigned"
}    
```

例如：

```json
{
    "apiVersion": "2019-09-07",
    "type": "Microsoft.Kusto/clusters",
    "name": "[variables('clusterName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "trustedExternalTenants": [],
        "virtualNetworkConfiguration": null,
        "optimizedAutoscale": null,
        "enableDiskEncryption": false,
        "enableStreamingIngest": false,
    }
}
```

创建群集后，它具有以下附加属性：

```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

`<TENANTID>` 和 `<PRINCIPALID>` 已替换为 GUID。 `TenantId` 属性标识该标识所属的 AAD 租户。 `PrincipalId` 是群集的新标识的唯一标识符。 在 AAD 中，服务主体的名称与你为应用服务或 Azure Functions 实例提供的名称相同。

## <a name="remove-an-identity"></a>删除标识

删除系统分配的标识也会将它从 AAD 中删除。 删除群集资源时，也会自动从 AAD 中删除系统分配的标识。 可以通过禁用该功能来删除系统分配的标识：

```json
"identity": {
    "type": "None"
}
```

## <a name="next-steps"></a>后续步骤

* [在 Azure 中保护 Azure 数据资源管理器群集](security.md)
* 通过启用静态加密[保护 Azure 数据资源管理器中的群集 - Azure 门户](manage-cluster-security.md)。
 * [使用 C# 配置客户管理的密钥](customer-managed-keys-csharp.md)
 * [使用 Azure 资源管理器模板配置客户管理的密钥](customer-managed-keys-resource-manager.md)
