---
title: 将资源部署到管理组
description: 介绍如何通过 Azure 资源管理器模板在管理组范围部署资源。
ms.topic: conceptual
origin.date: 11/07/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: 51763cfe5156ed8bd8e240aaa002ff051ab77be6
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631225"
---
# <a name="create-resources-at-the-management-group-level"></a>在管理组级别创建资源

通常情况下，你可将 Azure 资源部署到 Azure 订阅中的资源组。 但是，也可在管理组级别创建资源。 可以使用管理组级别部署来执行在该级别合理的操作，例如分配[基于角色的访问控制](../../role-based-access-control/overview.md)或应用[策略](../../governance/policy/overview.md)。

目前，若要在管理组级别部署模板，必须使用 REST API。

## <a name="supported-resources"></a>支持的资源

<!--Not Available on Microsoft Azure Template content -->

### <a name="schema"></a>架构

用于管理组部署的架构不同于资源组部署的架构。

对于模板，请使用：

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#
```

对于参数文件，请使用：

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentParameters.json#
```

## <a name="deployment-commands"></a>部署命令

用于管理组部署的命令不同于资源组部署的命令。

对于 REST API，请使用[部署 - 在管理组范围内创建](https://docs.microsoft.com/rest/api/resources/deployments/createorupdateatmanagementgroupscope)。

## <a name="deployment-location-and-name"></a>部署位置和名称

对于管理组级别部署，必须为部署提供位置。 部署位置独立于部署的资源的位置。 部署位置指定何处存储部署数据。

可以为部署提供一个名称，也可以使用默认部署名称。 默认名称是模板文件的名称。 例如，部署一个名为 **azuredeploy.json** 的模板将创建默认部署名称 **azuredeploy**。

每个部署名称的位置不可变。 当某个位置中已有某个部署时，无法在另一位置创建同名的部署。 如果出现错误代码 `InvalidDeploymentLocation`，请使用其他名称或使用与该名称的以前部署相同的位置。

## <a name="use-template-functions"></a>使用模板函数

对于管理组部署，在使用模板函数时有一些重要注意事项：

* 不支持 [resourceGroup()](template-functions-resource.md#resourcegroup) 函数。 
* **不**支持 [subscription()](template-functions-resource.md#subscription) 函数。
* 支持 [resourceId()](template-functions-resource.md#resourceid) 函数。 可以使用它获取在管理组级别部署中使用的资源的资源 ID。 例如，使用 `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))` 获取策略定义的资源 ID。 它返回 `/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}` 格式的资源 ID。
* 支持 [reference()](template-functions-resource.md#reference) 和 [list()](template-functions-resource.md#list) 函数。

## <a name="create-policies"></a>创建策略

### <a name="define-policy"></a>定义策略

以下示例展示如何在管理组级别[定义](../../governance/policy/concepts/definition-structure.md)策略。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
            "properties": {
                "policyType": "Custom",
                "parameters": {},
                "policyRule": {
                    "if": {
                        "field": "location",
                        "equals": "northeurope"
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        }
    ]
}
```

### <a name="assign-policy"></a>分配策略

以下示例将现有的策略定义分配到管理组。 如果策略使用参数，请将参数作为对象提供。 如果策略不使用参数，请使用默认的空对象。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "policyDefinitionID": {
            "type": "string"
        },
        "policyName": {
            "type": "string"
        },
        "policyParameters": {
            "type": "object",
            "defaultValue": {}
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```

## <a name="next-steps"></a>后续步骤

* 若要了解如何分配角色，请参阅[使用 RBAC 和 Azure 资源管理器模板管理对 Azure 资源的访问权限](../../role-based-access-control/role-assignments-template.md)。
* 若要通过示例来了解如何为 Azure 安全中心部署工作区设置，请参阅 [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json)。
* 若要了解有关创建 Azure Resource Manager模板的信息，请参阅[创作模板](template-syntax.md)。 
* 有关模板中的可用函数列表，请参阅[模板函数](template-functions.md)。

<!-- Update_Description: new article about deploy to management group -->
<!--NEW.date: 11/25/2019-->