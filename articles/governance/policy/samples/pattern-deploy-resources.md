---
title: 模式：使用策略定义部署资源
description: 此 Azure Policy 模式提供了有关如何使用策略定义部署资源的示例。
origin.date: 01/31/2020
ms.date: 03/09/2020
ms.author: v-tawe
ms.topic: sample
ms.openlocfilehash: c3fb2637d774bf31e5161f8dd90eaf5774d68116
ms.sourcegitcommit: 892137d117bcaf9d88aec0eb7ca756fe39613344
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78048864"
---
# <a name="azure-policy-pattern-deploy-resources"></a>Azure Policy 模式：部署资源

[deployIfNotExists](../concepts/effects.md#deployifnotexists) 效果使得在创建或更新不符合要求的资源时能够部署 [Azure 资源管理器模板](../../../azure-resource-manager/templates/overview.md)。 与使用 [deny](../concepts/effects.md#deny) 效果相比，此方法更好，因为它允许继续创建资源，并确保进行更改以使资源符合要求。

## <a name="sample-policy-definition"></a>示例策略定义

此策略定义使用 **field** 运算符来计算创建或更新的资源的 `type`。 当资源是 _Microsoft.Network/virtualNetworks_ 时，策略将在新资源或已更新资源的位置中查找网络观察程序。 如果找不到匹配的网络观察程序，则会部署资源管理器模板来创建缺少的资源。

```json
{
   "properties": {
       "displayName": "Deploy network watcher when virtual networks are created",
       "mode": "Indexed",
       "description": "This policy creates a network watcher resource in regions with virtual networks. You need to ensure existence of a resource group named networkWatcherRG, which will be used to deploy network watcher instances.",
       "metadata": {
           "category": "Network"
       },
       "parameters": {},
       "policyRule": {
           "if": {
               "field": "type",
               "equals": "Microsoft.Network/virtualNetworks"
           },
           "then": {
               "effect": "DeployIfNotExists",
               "details": {
                   "type": "Microsoft.Network/networkWatchers",
                   "resourceGroupName": "networkWatcherRG",
                   "existenceCondition": {
                       "field": "location",
                       "equals": "[field('location')]"
                   },
                   "roleDefinitionIds": [
                       "/providers/Microsoft.Authorization/roleDefinitions/4d97b98b-1d4f-4787-a291-c67834d212e7"
                   ],
                   "deployment": {
                       "properties": {
                           "mode": "incremental",
                           "template": {
                               "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
                               "contentVersion": "1.0.0.0",
                               "parameters": {
                                   "location": {
                                       "type": "string"
                                   }
                               },
                               "resources": [{
                                   "apiVersion": "2016-09-01",
                                   "type": "Microsoft.Network/networkWatchers",
                                   "name": "[concat('networkWacher_', parameters('location'))]",
                                   "location": "[parameters('location')]"
                               }]
                           },
                           "parameters": {
                               "location": {
                                   "value": "[field('location')]"
                               }
                           }
                       }
                   }
               }
           }
       }
   }
}
```

### <a name="explanation"></a>说明

#### <a name="existencecondition"></a>existenceCondition

```json
"type": "Microsoft.Network/networkWatchers",
"resourceGroupName": "networkWatcherRG",
"existenceCondition": {
   "field": "location",
   "equals": "[field('location')]"
},
```

**properties.policyRule.then.details** 块告诉 Azure Policy 要在 **properties.policyRule.if** 块中查找与创建或更新的资源相关的哪些内容。 在此示例中，资源组 **networkWatcherRG** 中必须存在 **field** `location` 等于新资源或已更新资源的位置的一个网络观察程序。 使用 `field()` 函数将允许 **existenceCondition** 访问新的或已更新资源上的属性，具体而言是 `location` 属性。

#### <a name="roledefinitionids"></a>roleDefinitionIds

```json
"roleDefinitionIds": [
   "/providers/Microsoft.Authorization/roleDefinitions/4d97b98b-1d4f-4787-a291-c67834d212e7"
],
```
**properties.policyRule.then.details** 块中的 **roleDefinitionIds** _array_ 属性告诉策略定义托管标识需要使用哪些权限来部署所包括的资源管理器模板。 必须设置此属性来包括具有模板部署所需权限的角色，但应使用“最小权限原则”的概念，只应包括必需的操作，不包括任何其他内容。

#### <a name="deployment-template"></a>部署模板

策略定义的 **deployment** 部分有一个 **properties** 块，其中定义了三个核心组件：

- **mode** - 此属性设置模板的[部署模式](../../../azure-resource-manager/templates/deployment-modes.md)。

- **template** - 此属性包括了模板本身。 在此示例中，**location** 模板参数设置新的网络观察程序资源的位置。

  ```json
  "template": {
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
   "contentVersion": "1.0.0.0",
   "parameters": {
       "location": {
           "type": "string"
       }
   },
   "resources": [{
       "apiVersion": "2016-09-01",
       "type": "Microsoft.Network/networkWatchers",
       "name": "[concat('networkWacher_', parameters('location'))]",
       "location": "[parameters('location')]"
   }]
  },
  ```
  
- **parameters** - 此属性定义提供给 **template** 的参数。 参数名称必须与在 **template** 中定义的名称匹配。 在此示例中，将此参数命名为 **location** 以便匹配。 **location** 的值再次使用 `field()` 函数来获取所计算资源（即 **policyRule.if** 块中的虚拟网络）的值。

  ```json
  "parameters": {
   "location": {
       "value": "[field('location')]"
   }
  }
  ```

## <a name="next-steps"></a>后续步骤

- 查看其他[模式和内置定义](./index.md)。
- 查看 [Azure Policy 定义结构](../concepts/definition-structure.md)。
- 查看[了解策略效果](../concepts/effects.md)。