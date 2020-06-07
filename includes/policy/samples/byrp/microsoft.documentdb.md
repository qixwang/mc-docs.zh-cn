---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 05/05/2020
ms.date: 06/08/2020
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: 6bb4ebdf6ee92d1f3ce20e65a1b4aae6887bed55
ms.sourcegitcommit: 8a2fbc0eae8d8f7297f5334f508ff868b4077f32
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2020
ms.locfileid: "84326267"
---
<!--Verified successfully-->

|名称 |说明 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[Azure Cosmos DB 允许的位置](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0473574d-2d43-4217-aefe-941fcdf7e684) |使用此策略可限制组织在部署 Azure Cosmos DB 资源时可指定的位置。 用于强制执行异地符合性要求。 |[parameters('policyEffect')] |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_Locations_Deny.json)。 |
|[Cosmos DB 应使用虚拟网络服务终结点](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe0a2b1a3-f7f9-4569-807f-2a9edebdf4d9) |此策略审核任何未配置为使用虚拟网络服务终结点的 Cosmos DB。 |Audit、Disabled |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetworkServiceEndpoint_CosmosDB_Audit.json)。 |

<!--Not Available on |[Deploy Advanced Threat Protection for Cosmos DB Accounts](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb5f04e03-92a3-4b09-9410-2cc5e5047656) |This policy enables Advanced Threat Protection across Cosmos DB accounts. |DeployIfNotExists, Disabled |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/CosmosDbAdvancedThreatProtection_Deploy.json) |-->

<!-- Update_Description: new article about policy samples -->
<!--NEW.date: 06/15/2020-->