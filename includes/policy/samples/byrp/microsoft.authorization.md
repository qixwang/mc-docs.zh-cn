---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/25/2020
ms.author: v-junlch
ms.custom: generated
ms.openlocfilehash: 22f52f56efde5215e2732a14b897475690c6ce22
ms.sourcegitcommit: 7429daf26cff014b040f69cdae75bdeaea4f4e93
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2020
ms.locfileid: "83991928"
---
|名称 |说明 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[审核自定义 RBAC 规则的使用情况](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa451c1ef-c6ca-483d-87ed-f49761e3ffb5) |审核“所有者、参与者、读者”等内置角色而不是容易出错的自定义 RBAC 角色。 使用自定义角色被视为例外，需要进行严格的审查和威胁建模 |Audit, 已禁用 |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/Subscription_AuditCustomRBACRoles_Audit.json)。 |
|[不应存在自定义订阅所有者角色](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F10ee2ea2-fb4d-45b8-a7e9-a2e770044cd9) |此策略确保不存在自定义订阅所有者角色。 |Audit, 已禁用 |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json)。 |

