---
title: 模式：在策略定义中使用标记
description: 此 Azure Policy 模式提供有关如何在策略定义中添加参数化标记或从资源组继承标记的示例。
origin.date: 05/20/2020
ms.date: 08/06/2020
ms.author: v-tawe
ms.topic: sample
ms.openlocfilehash: 0acaeb1c3b139519816d2f19e63dbfeac6cf8340
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919227"
---
# <a name="azure-policy-pattern-tags"></a>Azure Policy 模式：标记

[标记](../../../azure-resource-manager/management/tag-resources.md)是管理、组织和控制 Azure 资源的重要组成部分。 利用 Azure Policy，可以通过[修改](../concepts/effects.md#modify)效果和[修正任务](../how-to/remediate-resources.md)在新资源和现有资源上大规模配置标记。

## <a name="sample-1-parameterize-tags"></a>示例 1：参数化标记

此策略定义使用两个参数（tagName 和 tagValue）来设置策略分配在资源组上查找的内容。 此格式允许将策略定义用于任意数量的标记名称和标记值组合，但只保留单个策略定义。

> [!NOTE]
> 尽管此策略定义模式类似于[模式：参数 - 示例 #1](./pattern-parameters.md#sample-1-string-parameters) 中的策略定义模式，但此示例使用 All 模式并以资源组为目标。

```json
{
    "properties": {
        "displayName": "Add or replace a tag on resource groups",
        "mode": "All",
        "description": "Adds or replaces the specified tag and value when any resource group is created or updated. Existing resource groups can be remediated by triggering a remediation task.",
        "metadata": {
            "category": "Tags"
        },
        "parameters": {
            "tagName": {
                "type": "String",
                "metadata": {
                    "displayName": "Tag Name",
                    "description": "Name of the tag, such as 'environment'"
                }
            },
            "tagValue": {
                "type": "String",
                "metadata": {
                    "displayName": "Tag Value",
                    "description": "Value of the tag, such as 'production'"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [{
                        "field": "type",
                        "equals": "Microsoft.Resources/subscriptions/resourceGroups"
                    },
                    {
                        "field": "[concat('tags[', parameters('tagName'), ']')]",
                        "notEquals": "[parameters('tagValue')]"
                    }
                ]
            },
            "then": {
                "effect": "modify",
                "details": {
                    "roleDefinitionIds": [
                        "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
                    ],
                    "operations": [{
                        "operation": "addOrReplace",
                        "field": "[concat('tags[', parameters('tagName'), ']')]",
                        "value": "[parameters('tagValue')]"
                    }]
                }
            }
        }
    }
}
```

### <a name="sample-1-explanation"></a>示例 1：说明

```json
"properties": {
    "displayName": "Add or replace a tag on resource groups",
    "mode": "All",
    "description": "Adds or replaces the specified tag and value when any resource group is created or updated. Existing resource groups can be remediated by triggering a remediation task.",
    "metadata": {
        "category": "Tags"
    },
```

在此示例中，“模式”设置为“All”，因为它以资源组为目标。 在大多数情况下，使用标记时，“模式”应设置为“已编入索引”。 有关详细信息，请参阅[模式](../concepts/definition-structure.md#resource-manager-modes)。

```json
"if": {
    "allOf": [{
            "field": "type",
            "equals": "Microsoft.Resources/subscriptions/resourceGroups"
        },
        {
            "field": "[concat('tags[', parameters('tagName'), ']')]",
            "notEquals": "[parameters('tagValue')]"
        }
    ]
},
```

在策略定义的此部分，`concat` 组合参数化的“tagName”参数和 `tags['name']` 格式，以指示“字段”为参数“tagValue”对该标记求值。
由于使用了 notEquals，如果 tags\[tagName\] 不等于 tagValue，则会触发 modify 效果。

```json
"operations": [{
    "operation": "addOrReplace",
    "field": "[concat('tags[', parameters('tagName'), ']')]",
    "value": "[parameters('tagValue')]"
}]
```

此处的 addOrReplace 操作使用与使用参数化标记值相同的格式，在已求值资源组上将标记创建或更新为所需的值。

## <a name="sample-2-inherit-tag-value-from-resource-group"></a>示例 2：从资源组继承标记值

此策略定义使用参数 tagName 来确定要从父资源组继的标记的值。

```json
{
    "properties": {
        "displayName": "Inherit a tag from the resource group",
        "mode": "Indexed",
        "description": "Adds or replaces the specified tag and value from the parent resource group when any resource is created or updated. Existing resources can be remediated by triggering a remediation task.",
        "metadata": {
            "category": "Tags"
        },
        "parameters": {
            "tagName": {
                "type": "String",
                "metadata": {
                    "displayName": "Tag Name",
                    "description": "Name of the tag, such as 'environment'"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [{
                        "field": "[concat('tags[', parameters('tagName'), ']')]",
                        "notEquals": "[resourceGroup().tags[parameters('tagName')]]"
                    },
                    {
                        "value": "[resourceGroup().tags[parameters('tagName')]]",
                        "notEquals": ""
                    }
                ]
            },
            "then": {
                "effect": "modify",
                "details": {
                    "roleDefinitionIds": [
                        "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
                    ],
                    "operations": [{
                        "operation": "addOrReplace",
                        "field": "[concat('tags[', parameters('tagName'), ']')]",
                        "value": "[resourceGroup().tags[parameters('tagName')]]"
                    }]
                }
            }
        }
    }
}
```

### <a name="sample-2-explanation"></a>示例 2：说明

```json
"properties": {
    "displayName": "Inherit a tag from the resource group",
    "mode": "Indexed",
    "description": "Adds or replaces the specified tag and value from the parent resource group when any resource is created or updated. Existing resources can be remediated by triggering a remediation task.",
    "metadata": {
        "category": "Tags"
    },
```

在此示例中，“模式”设置为“已编入索引”，因为即使它从资源组获取值，也不会以资源组或订阅为目标。 有关详细信息，请参阅[模式](../concepts/definition-structure.md#resource-manager-modes)。

```json
"if": {
    "allOf": [{
            "field": "[concat('tags[', parameters('tagName'), ']')]",
            "notEquals": "[resourceGroup().tags[parameters('tagName')]]"
        },
        {
            "value": "[resourceGroup().tags[parameters('tagName')]]",
            "notEquals": ""
        }
    ]
},
```

policyRule.if 使用 `concat`（如 [示例 #1](#sample-1-parameterize-tags)）来计算 tagName 的值，但使用 `resourceGroup()` 函数将其与父资源组上相同标记的值进行比较。 此处的第二个子句检查资源组上的标记是否具有值并且不为 null。

```json
"operations": [{
    "operation": "addOrReplace",
    "field": "[concat('tags[', parameters('tagName'), ']')]",
    "value": "[resourceGroup().tags[parameters('tagName')]]"
}]
```

此处要分配给资源上的 tagName 标记的值还使用 `resourceGroup()` 函数从父资源组获取值。 这样一来，你可以从父资源组继承标记。 如果你已创建资源但未添加标记，则相同的策略定义和[修复任务](../how-to/remediate-resources.md)可以更新现有资源。

## <a name="next-steps"></a>后续步骤

- 查看其他[模式和内置定义](./index.md)。
- 查看 [Azure Policy 定义结构](../concepts/definition-structure.md)。
- 查看[了解策略效果](../concepts/effects.md)。