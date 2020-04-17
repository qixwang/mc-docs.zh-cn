---
title: 模式：策略定义中的参数
description: 此 Azure Policy 模式通过示例介绍了如何在策略定义中使用参数。
origin.date: 01/31/2020
ms.date: 03/09/2020
ms.author: v-tawe
ms.topic: sample
ms.openlocfilehash: 9a9c78dd9673d49fdb663718281f64598e0c6d98
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78048870"
---
# <a name="azure-policy-pattern-parameters"></a>Azure Policy 模式：参数

可以将策略定义设置为动态，减少使用[参数](../concepts/definition-structure.md#parameters)所需的策略定义的数量。 参数在策略分配过程中定义。 参数有一组预定义的属性，这些属性描述参数及其使用方式。

## <a name="sample-1-string-parameters"></a>示例 1：字符串参数

此策略定义使用两个参数（**tagName** 和 **tagValue**）来设置策略分配在资源上查找的内容。 此格式允许将策略用于任意数量的标记名称和标记值组合，但只保留单个策略定义。

```json
{
   "properties": {
       "displayName": "Require tag and its value",
       "policyType": "BuiltIn",
       "mode": "Indexed",
       "description": "Enforces a required tag and its value. Does not apply to resource groups.",
       "parameters": {
           "tagName": {
               "type": "String",
               "metadata": {
                   "description": "Name of the tag, such as costCenter"
               }
           },
           "tagValue": {
               "type": "String",
               "metadata": {
                   "description": "Value of the tag, such as headquarter"
               }
           }
       },
       "policyRule": {
           "if": {
               "not": {
                   "field": "[concat('tags[', parameters('tagName'), ']')]",
                   "equals": "[parameters('tagValue')]"
               }
           },
           "then": {
               "effect": "deny"
           }
       }
   }
}
```

### <a name="sample-1-explanation"></a>示例 1：说明

```json
"tagName": {
   "type": "String",
   "metadata": {
       "description": "Name of the tag, such as costCenter"
   }
},
```

在策略定义的此部分，我们将 **tagName** 参数定义为 _string_，并提供说明，方便其使用。

然后，我们在 **policyRule.if** 块中使用该参数，将策略设置为动态。 在这里，它用于定义所计算的字段，该字段是值为 **tagName** 的标记。

```json
"if": {
   "not": {
       "field": "[concat('tags[', parameters('tagName'), ']')]",
       "equals": "[parameters('tagValue')]"
   }
},
```

## <a name="sample-2-array-parameters"></a>示例 2：数组参数

此策略定义使用单个参数 (**listOfBandwidthinMbps**)，目的是检查 Express Route 线路资源是否已将带宽设置配置为已批准的值之一。 如果它不匹配，系统会[拒绝](../concepts/effects.md#deny)创建或更新资源。

```json
{
   "properties": {
       "displayName": "Allowed Express Route bandwidth",
       "description": "This policy enables you to specify a set of express route bandwidths that your organization can deploy.",
       "parameters": {
           "listOfBandwidthinMbps": {
               "type": "Array",
               "metadata": {
                   "description": "The list of SKUs that can be specified for express route.",
                   "displayName": "Allowed Bandwidth"
               }
           }
       },
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Network/expressRouteCircuits"
                   },
                   {
                       "not": {
                           "field": "Microsoft.Network/expressRouteCircuits/serviceProvider.bandwidthInMbps",
                           "in": "[parameters('listOfBandwidthinMbps')]"
                       }
                   }
               ]
           },
           "then": {
               "effect": "Deny"
           }
       }
   }
}
```

### <a name="sample-2-explanation"></a>示例 2：说明

```json
"listOfBandwidthinMbps": {
   "type": "Array",
   "metadata": {
       "description": "The list of SKUs that can be specified for express route.",
       "displayName": "Allowed Bandwidth"
   }
}
```

在策略定义的此部分，我们将 **listOfBandwidthinMbps** 参数定义为 _array_，并提供说明，方便其使用。 作为 _array_，它有多个要匹配的值。

然后，我们在 **policyRule.if** 块中使用该参数。 作为 _array_ 参数，必须使用 _array_
[条件](../concepts/definition-structure.md#conditions)的 **in** 或 **notIn**。
在这里，我们将其用于 **serviceProvider.bandwidthInMbps** 别名，作为定义的值之一。

```json
"not": {
   "field": "Microsoft.Network/expressRouteCircuits/serviceProvider.bandwidthInMbps",
   "in": "[parameters('listOfBandwidthinMbps')]"
}
```

## <a name="next-steps"></a>后续步骤

- 查看其他[模式和内置定义](./index.md)。
- 查看 [Azure Policy 定义结构](../concepts/definition-structure.md)。
- 查看[了解策略效果](../concepts/effects.md)。