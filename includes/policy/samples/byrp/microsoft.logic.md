---
author: rockboyfor
ms.service: azure-policy
ms.topic: include
origin.date: 05/05/2020
ms.date: 06/08/2020
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: b3bd62129292c8da8f77a400cea88253444254f7
ms.sourcegitcommit: 8a2fbc0eae8d8f7297f5334f508ff868b4077f32
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2020
ms.locfileid: "84326242"
---
|名称 |说明 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[将逻辑应用的诊断设置部署到事件中心](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa1dae6c7-13f3-48ea-a149-ff8442661f60) |在创建或更新缺少此诊断设置的任何逻辑应用时，为逻辑应用部署诊断设置以将其流式传输到区域事件中心。 |DeployIfNotExists、Disabled |2.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/LogicApps_DeployDiagnosticLog_Deploy_EventHub.json)。 |
|[将逻辑应用的诊断设置部署到 Log Analytics 工作区](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb889a06c-ec72-4b03-910a-cb169ee18721) |在创建或更新缺少此诊断设置的任何逻辑应用时，为逻辑应用部署诊断设置以将其流式传输到区域 Log Analytics 工作区。 |DeployIfNotExists、Disabled |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/LogicApps_DeployDiagnosticLog_Deploy_LogAnalytics.json)。 |
|[应启用逻辑应用的诊断日志](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34f95f76-5386-4de7-b824-0d8478470c9d) |审核是否已启用诊断日志。 这样便可以在发生安全事件或网络受到威胁时重新创建活动线索以用于调查目的 |AuditIfNotExists, 已禁用 |2.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Logic%20Apps/LogicApps_AuditDiagnosticLog_Audit.json)。 |

<!-- Update_Description: new article about policy samples -->
<!--NEW.date: 06/15/2020-->