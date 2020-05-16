---
title: 将资源部署到租户
description: 介绍如何通过 Azure 资源管理器模板在租户范围内部署资源。
ms.topic: conceptual
origin.date: 03/16/2020
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: 600013a67639c315664a7256e2925b9278fec9f6
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002260"
---
<!--Verified successfully-->
# <a name="create-resources-at-the-tenant-level"></a>在租户级别创建资源

随着组织的成熟，你可能需要在 Azure AD 租户中定义和分配[策略](../../governance/policy/overview.md)或[基于角色的访问控制](../../role-based-access-control/overview.md)。 使用租户级别模板，你可以以声明方式应用策略并在全局级别分配角色。

## <a name="supported-resources"></a>支持的资源

可以在租户级别部署以下资源类型：

* [部署](https://docs.microsoft.com/azure/templates/microsoft.resources/deployments) - 适用于那些部署到管理组或订阅的嵌套模板。
* [policyAssignments](https://docs.microsoft.com/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](https://docs.microsoft.com/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](https://docs.microsoft.com/azure/templates/microsoft.authorization/policysetdefinitions)
* [roleAssignments](https://docs.microsoft.com/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](https://docs.microsoft.com/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>架构

用于租户部署的架构不同于资源组部署的架构。

对于模板，请使用：

```json
https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#
```

对于所有部署范围，参数文件的架构都相同。 对于参数文件，请使用：

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
```

## <a name="required-access"></a>所需访问权限

部署模板的主体必须有权在租户范围内创建资源。 主体必须有权执行部署操作 (`Microsoft.Resources/deployments/*`) 和创建模板中定义的资源。 例如，若要创建管理组，主体必须在租户范围内具有“参与者”权限。 若要创建角色分配，主体必须具有“所有者”权限。

Azure Active Directory 的全局管理员不会自动具有分配角色的权限。 若要在租户范围内启用模板部署，全局管理员必须执行以下步骤：

1. 提升帐户访问权限，使全局管理员可以分配角色。 有关详细信息，请参阅[提升访问权限以管理所有 Azure 订阅和管理组](../../role-based-access-control/elevate-access-global-admin.md)。

1. 向需要部署模板的主体分配“所有者”或“参与者”角色。

    ```powershell
    New-AzRoleAssignment -SignInName "[userId]" -Scope "/" -RoleDefinitionName "Owner"
    ```

    ```azurecli
    az role assignment create --assignee "[userId]" --scope "/" --role "Owner"
    ```

主体现在具有部署模板所需的权限。

## <a name="deployment-commands"></a>部署命令

租户部署的命令与资源组部署的命令不同。

对于 Azure CLI，请使用 [az deployment tenant create](https://docs.microsoft.com/cli/azure/deployment/tenant?view=azure-cli-latest#az-deployment-tenant-create)：

```azurecli
az deployment tenant create \
  --name demoTenantDeployment \
  --location ChinaNorth \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-level-deployments/new-mg/azuredeploy.json"
```

对于 Azure PowerShell，请使用 [New-AzTenantDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-aztenantdeployment)。

```powershell
New-AzTenantDeployment `
  -Name demoTenantDeployment `
  -Location "China North" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-level-deployments/new-mg/azuredeploy.json"
```

对于 REST API，请参阅 [Deployments - Create Or Update At Tenant Scope](https://docs.microsoft.com/rest/api/resources/deployments/createorupdateattenantscope)（部署 - 在租户范围内创建或更新）。

## <a name="deployment-location-and-name"></a>部署位置和名称

对于租户级别部署，必须为部署提供位置。 部署位置独立于部署的资源的位置。 部署位置指定何处存储部署数据。

可以为部署提供一个名称，也可以使用默认部署名称。 默认名称是模板文件的名称。 例如，部署一个名为 **azuredeploy.json** 的模板将创建默认部署名称 **azuredeploy**。

每个部署名称的位置不可变。 当某个位置中已有某个部署时，无法在另一位置创建同名的部署。 如果出现错误代码 `InvalidDeploymentLocation`，请使用其他名称或使用与该名称的以前部署相同的位置。

## <a name="use-template-functions"></a>使用模板函数

对于租户部署，在使用模板函数时有一些重要注意事项：

* 不支持 [resourceGroup()](template-functions-resource.md#resourcegroup) 函数。 
* **不**支持 [subscription()](template-functions-resource.md#subscription) 函数。
* 支持 [reference()](template-functions-resource.md#reference) 和 [list()](template-functions-resource.md#list) 函数。
* 使用 [tenantResourceId()](template-functions-resource.md#tenantresourceid) 函数获取在租户级别部署的资源的资源 ID。

    例如，若要获取策略定义的资源 ID，请使用：

    ```json
    tenantResourceId('Microsoft.Authorization/policyDefinitions/', parameters('policyDefinition'))
    ```

    返回的资源 ID 具有以下格式：

    ```json
    /providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
    ```

## <a name="create-management-group"></a>创建管理组

[以下模板](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-level-deployments/new-mg)创建管理组。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "mgName": {
      "type": "string",
      "defaultValue": "[concat('mg-', uniqueString(newGuid()))]"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Management/managementGroups",
      "apiVersion": "2019-11-01",
      "name": "[parameters('mgName')]",
      "properties": {
      }
    }
  ]
}
```

## <a name="assign-role"></a>分配角色

[以下模板](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-level-deployments/tenant-role-assignment)在租户范围内分配角色。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "principalId if the user that will be given contributor access to the resourceGroup"
      }
    },
    "roleDefinitionId": {
      "type": "string",
      "defaultValue": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
      "metadata": {
        "description": "roleDefinition for the assignment - default is owner"
      }
    }
  },
  "variables": {
    // This creates an idempotent guid for the role assignment
    "roleAssignmentName": "[guid('/', parameters('principalId'), parameters('roleDefinitionId'))]"
  },
  "resources": [
    {
      "name": "[variables('roleAssignmentName')]",
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2019-04-01-preview",
      "properties": {
        "roleDefinitionId": "[tenantResourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
        "principalId": "[parameters('principalId')]",
        "scope": "/"
      }
    }
  ]
}
```

## <a name="next-steps"></a>后续步骤

* 若要了解如何分配角色，请参阅[使用 RBAC 和 Azure 资源管理器模板管理对 Azure 资源的访问权限](../../role-based-access-control/role-assignments-template.md)。
* 还可以在[订阅级别](deploy-to-subscription.md)或[管理组级别](deploy-to-management-group.md)部署模板。

<!-- Update_Description: new article about deploy to tenant -->
<!--NEW.date: 05/06/2020-->