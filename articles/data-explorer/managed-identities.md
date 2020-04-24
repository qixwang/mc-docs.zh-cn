---
title: 如何配置 Azure 数据资源管理器群集的托管标识
description: 了解如何配置 Azure 数据资源管理器群集的托管标识。
author: saguiitay
ms.author: v-tawe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
origin.date: 03/12/2020
ms.date: 03/30/2020
ms.openlocfilehash: a495e5ae3519fcd85d6553ea1953cbef8510096a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80522084"
---
# <a name="configure-managed-identities-for-your-azure-data-explorer-cluster"></a>配置 Azure 数据资源管理器群集的托管标识

借助 [Azure Active Directory 中的托管标识](/active-directory/managed-identities-azure-resources/overview)，群集可以轻松访问其他受 AAD 保护的资源（例如 Azure Key Vault）。 标识由 Azure 平台托管，无需预配或轮换任何机密。 本文介绍如何为 Azure 数据资源管理器群集创建托管标识。 当前支持托管标识配置只是为了[为群集启用客户管理的密钥](/data-explorer/security#customer-managed-keys-with-azure-key-vault)。

> [!Note]
> 如果在订阅或租户之间迁移了应用，Azure 数据资源管理器的托管标识将不按预期方式运行。 应用需要获取新标识，这可以通过[禁用](#remove-a-system-assigned-identity)并[重新启用](#add-a-system-assigned-identity)该功能来完成。 还需要更新下游资源的访问策略才能使用新标识。

## <a name="add-a-system-assigned-identity"></a>添加系统分配的标识

分配一个系统分配的标识，该标识绑定到群集，在群集被删除时删除。 一个群集只能有一个系统分配的标识。 使用系统分配的标识创建群集需要在该群集上设置一个额外的属性。 系统分配的标识使用 C#、ARM 模板或 Azure 门户添加，详述如下。

# <a name="azure-portal"></a>[Azure 门户](#tab/portal)

### <a name="add-a-system-assigned-identity-using-the-azure-portal"></a>使用 Azure 门户添加系统分配的标识

1. 登录 [Azure 门户](https://portal.azure.cn/)。

#### <a name="new-azure-data-explorer-cluster"></a>新建 Azure 数据资源管理器群集

1. [创建 Azure 数据资源管理器群集](/data-explorer/create-cluster-database-portal#create-a-cluster) 
1. 在“安全性”选项卡>“系统分配的标识”中，选择“打开”。    若要删除系统分配的标识，请选择“关闭”  。
2. 选择“下一步: 标记>”或“查看 + 创建”，创建此群集  。 

    ![将系统分配的标识添加到新群集](media/managed-identities/system-assigned-identity-new-cluster.png)

#### <a name="existing-azure-data-explorer-cluster"></a>现有的 Azure 数据资源管理器群集

1. 打开现有的 Azure 数据资源管理器群集。
1. 在门户的左窗格中，选择“设置” **“标识”。**  >  
1. 在“标识”窗格的“系统分配”选项卡中，执行以下操作：  
   1. 将“状态”滑块移到“打开”。  
   1. 选择“保存” 
   1. 在弹出窗口中选择“是” 

    ![添加系统分配标识](media/managed-identities/turn-system-assigned-identity-on.png)

1. 几分钟后，屏幕显示： 
  * **对象 ID** - 用于客户托管密钥 
  * **角色分配** - 单击用于分配相关角色的链接

    ![系统分配标识处于打开状态](media/managed-identities/system-assigned-identity-on.png)

# <a name="c"></a>[C#](#tab/c-sharp)

### <a name="add-a-system-assigned-identity-using-c"></a>使用 C# 添加系统分配的标识

#### <a name="prerequisites"></a>必备条件

若要使用 Azure 数据资源管理器 C# 客户端设置托管标识，请执行以下操作：

* 安装 [Azure 数据资源管理器 (Kusto) NuGet 包](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)。
* 安装 [Microsoft.IdentityModel.Clients.ActiveDirectory NuGet 包](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)以进行身份验证。
* [创建可访问资源的 Azure AD 应用程序](/active-directory/develop/howto-create-service-principal-portal)和服务主体。 我们在订阅范围添加角色分配，并获取所需的 `Directory (tenant) ID`、`Application ID` 和 `Client Secret`。

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

    如果结果中包含具有 `ProvisioningState` 值的 `Succeeded`，则表示已创建或更新群集，群集应具有以下属性：

    ```csharp
    var principalId = cluster.Identity.PrincipalId;
    var tenantId = cluster.Identity.TenantId;
    ```

`PrincipalId` 和 `TenantId` 已替换为 GUID。 `TenantId` 属性标识该标识所属的 AAD 租户。 `PrincipalId` 是群集的新标识的唯一标识符。 在 AAD 中，服务主体的名称与你为应用服务或 Azure Functions 实例提供的名称相同。

# <a name="arm-template"></a>[ARM 模板](#tab/arm)

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

---

## <a name="remove-a-system-assigned-identity"></a>删除系统分配的标识

删除系统分配的标识也会将它从 AAD 中删除。 删除群集资源时，也会自动从 AAD 中删除系统分配的标识。 可以通过禁用该功能来删除系统分配的标识。  系统分配的标识使用 C#、ARM 模板或 Azure 门户删除，详述如下。

# <a name="azure-portal"></a>[Azure 门户](#tab/portal)

### <a name="remove-a-system-assigned-identity-using-the-azure-portal"></a>使用 Azure 门户删除系统分配的标识

1. 登录 [Azure 门户](https://portal.azure.cn/)。
1. 在门户的左窗格中，选择“设置” **“标识”。**  >  
1. 在“标识”窗格的“系统分配”选项卡中，执行以下操作：  
    1. 将“状态”滑块移到“关闭”。  
    1. 选择“保存” 
    1. 在弹出窗口中选择“是”，禁用系统分配的标识。  “标识”  窗格恢复到与添加系统分配标识之前相同的状况。

    ![系统分配标识处于关闭状态](media/managed-identities/system-assigned-identity.png)

# <a name="c"></a>[C#](#tab/c-sharp)

### <a name="remove-a-system-assigned-identity-using-c"></a>使用 C# 删除系统分配的标识

运行以下代码，删除系统分配的标识：

```csharp
var identity = new Identity(IdentityType.None);
var cluster = new Cluster(location, sku, identity: identity);
await kustoManagementClient.Clusters.CreateOrUpdateAsync(resourceGroupName, clusterName, cluster);
```

# <a name="arm-template"></a>[ARM 模板](#tab/arm)

### <a name="remove-a-system-assigned-identity-using-an-azure-resource-manager-template"></a>使用 Azure 资源管理器模板删除系统分配的标识

运行以下代码，删除系统分配的标识：

```json
"identity": {
    "type": "None"
}
```

---

## <a name="next-steps"></a>后续步骤

* [在 Azure 中保护 Azure 数据资源管理器群集](security.md)
* 通过启用静态加密[保护 Azure 数据资源管理器中的群集 - Azure 门户](manage-cluster-security.md)。

<!-- * [Configure customer-managed-keys using the Azure Resource Manager template](customer-managed-keys-resource-manager.md) -->
<!-- * [Configure customer-managed-keys using C#](customer-managed-keys-csharp.md) -->