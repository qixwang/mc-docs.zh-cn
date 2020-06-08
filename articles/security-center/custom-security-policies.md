---
title: 在 Azure 安全中心创建自定义安全策略
description: 通过 Azure 安全中心监视的 Azure 自定义策略定义。
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 05/14/2020
ms.author: v-tawe
origin.date: 03/25/2020
ms.openlocfilehash: 5f830b6307fd24e3a3f30c43ad9751cb5efae33e
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126747"
---
# <a name="using-custom-security-policies"></a>使用自定义安全策略

为了帮助保护你的系统和环境，Azure 安全中心会生成安全建议。 这些建议基于行业最佳做法，后者被纳入提供给所有客户的通用默认安全策略中。 它们也可能来自安全中心对行业标准和法规标准的了解。

使用此功能，你可以添加自己的自定义计划。 这样一来，如果你的环境不遵循你创建的策略，你就会收到安全建议。 你创建的任何自定义计划都将与内置计划一起显示在[提高合规性](security-center-compliance-dashboard.md)教程所述的合规性仪表板中。

如 [Azure Policy 文档](https://docs.azure.cn/governance/policy/concepts/definition-structure#definition-location)所述，当你为自定义计划指定位置时，该位置必须是管理组或订阅。 

## <a name="to-add-a-custom-initiative-to-your-subscription"></a>将自定义计划添加到订阅 

1. 从安全中心边栏中打开“安全策略”页面。

1. 选择要向其添加自定义计划的订阅或管理组。

    [![选择要为其创建自定义策略的订阅](media/custom-security-policies/custom-policy-selecting-a-subscription.png)](media/custom-security-policies/custom-policy-selecting-a-subscription.png#lightbox)

    > [!NOTE]
    > 必须在订阅级别（或更高级别）添加自定义标准，才能在安全中心评估并显示它们。 
    >
    > 添加自定义标准时，它会为该范围分配一个计划。 因此，建议选择该分配所需的最大范围。

1. 在“安全策略”页面的“你的自定义计划”下，单击“添加自定义计划”。

    [![单击“添加自定义计划”****](media/custom-security-policies/custom-policy-add-initiative.png)](media/custom-security-policies/custom-policy-add-initiative.png#lightbox)

    随即显示以下页面：

    ![创建或添加策略](media/custom-security-policies/create-or-add-custom-policy.png)

1. 在“添加自定义计划”页面中，查看组织中已创建的自定义策略的列表。 如果看到要分配给订阅的计划，请单击“添加”。 如果列表中没有符合需求的计划，请跳过此步骤。

1. 创建新的自定义计划：

    1. 单击“新建”。
    1. 输入定义的位置和名称。
    1. 选择要包括的策略，然后单击“添加”。
    1. 输入所需的任何参数。
    1. 单击“保存” 。
    1. 在“添加自定义计划”页面中，单击“刷新”。 新计划将显示为“可用”。
    1. 单击“添加”并将其分配给订阅。

    > [!NOTE]
    > 创建新计划需要有订阅所有者凭据。 有关 Azure 角色的详细信息，请参阅 [Azure 安全中心的权限](security-center-permissions.md)。

    你的新计划已生效，可以通过以下两种方式查看其影响：

    * 在安全中心边栏的“策略和符合性”下，选择“合规性”。 系统将打开合规性仪表板，其中显示了新的自定义计划和内置计划。
    
    * 如果环境不遵循所定义的策略，你将开始收到安全建议。

1. 若要查看针对你的策略生成的建议，请单击边栏中的“建议”，打开建议页面。 建议将显示“自定义”标签，并在大约一小时内可用。

    [![自定义建议](media/custom-security-policies/custom-policy-recommendations.png)](media/custom-security-policies/custom-policy-recommendations-in-context.png#lightbox)

## <a name="enhancing-your-custom-recommendations-with-detailed-information"></a>利用详细信息增强你的自定义建议

Azure 安全中心提供的内置建议包括严重性级别和修正说明等详细信息。 如果要将此类型的信息添加到自定义建议，使其显示在 Azure 门户或提供建议的任何位置，则需要使用 REST API。 

可以添加以下两类信息：

- **RemediationDescription** - 字符串
- **Severity** - 枚举 [Low、Medium、High]

应将元数据添加到策略的策略定义中，该策略应属于自定义计划的一部分。 它应位于“securityCenter”属性中，如下所示：

```json
 "metadata": {
    "securityCenter": {
        "RemediationDescription": "Custom description goes here",
        "Severity": "High",
    },
```

以下是包含 metadata/securityCenter 属性的自定义策略的示例：

  ```json
  {
"properties": {
    "displayName": "Security - ERvNet - AuditRGLock",
    "policyType": "Custom",
    "mode": "All",
    "description": "Audit required resource groups lock",
    "metadata": {
        "securityCenter": {
            "remediationDescription": "Resource Group locks can be set via Azure Portal -> Resource Group -> Locks",
            "severity": "High",
        },
    },
    "parameters": {
        "expressRouteLockLevel": {
            "type": "String",
            "metadata": {
                "displayName": "Lock level",
                "description": "Required lock level for ExpressRoute resource groups."
            },
            "allowedValues": [
                "CanNotDelete",
                "ReadOnly"
            ]
        }
    },
    "policyRule": {
        "if": {
            "field": "type",
            "equals": "Microsoft.Resources/subscriptions/resourceGroups"
        },
        "then": {
            "effect": "auditIfNotExists",
            "details": {
                "type": "Microsoft.Authorization/locks",
                "existenceCondition": {
                    "field": "Microsoft.Authorization/locks/level",
                    "equals": "[parameters('expressRouteLockLevel')]"
                }
            }
        }
    }
}
}
  ```

有关使用 securityCenter 属性的另一个示例，请参阅 [REST API 文档的此部分](https://docs.microsoft.com/rest/api/securitycenter/assessmentsmetadata/createinsubscription#examples)。


## <a name="next-steps"></a>后续步骤

本文介绍了如何创建自定义安全策略。 

其他相关材料，请参阅以下文章： 

- [安全策略概述](tutorial-security-policy.md)
- [内置安全策略列表](security-center-policy-definitions.md)