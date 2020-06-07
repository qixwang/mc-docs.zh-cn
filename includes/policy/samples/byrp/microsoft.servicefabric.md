---
author: rockboyfor
ms.service: azure-policy
ms.topic: include
origin.date: 05/05/2020
ms.date: 06/08/2020
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: bd41531003a9ddbfe3d6d5b275d1e8910374c5b2
ms.sourcegitcommit: 8a2fbc0eae8d8f7297f5334f508ff868b4077f32
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2020
ms.locfileid: "84326249"
---
|名称 |说明 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[Service Fabric 群集应将 ClusterProtectionLevel 属性设置为 EncryptAndSign](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F617c02be-7f02-4efd-8836-3180d47b6c68) |Service Fabric 使用主要群集证书为节点之间的通信提供三个保护级别（None、Sign 和 EncryptAndSign）。 设置保护级别以确保所有节点到节点消息均已进行加密和数字签名 |Audit、Deny、Disabled |1.1.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditClusterProtectionLevel_Audit.json)。 |
|[Service Fabric 群集应仅使用 Azure Active Directory 进行客户端身份验证](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb54ed75b-3e1a-44ac-a333-05ba39b99ff0) |审核 Service Fabric 中仅通过 Azure Active Directory 进行客户端身份验证 |Audit、Deny、Disabled |1.1.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditADAuth_Audit.json)。 |

<!-- Update_Description: new article about policy samples -->
<!--NEW.date: 06/15/2020-->