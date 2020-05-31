---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 05/05/2020
ms.date: 05/29/2020
ms.author: v-tawe
ms.custom: generated
ms.openlocfilehash: c8c7e3195225c1441ade3c4e14ee562ecb74a416
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199295"
---
|名称 |说明 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[应启用 Batch 帐户的诊断日志](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F428256e6-1fac-4f48-a757-df34c2b3336d) |审核是否已启用诊断日志。 这样便可以在发生安全事件或网络受到威胁时重新创建活动线索以用于调查目的 |AuditIfNotExists, 已禁用 |2.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditDiagnosticLog_Audit.json)
|[应针对 Batch 帐户配置指标警报规则](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F26ee67a2-f81a-4ba8-b9ce-8550bd5ee1a7) |审核是否已针对 Batch 帐户配置指标警报规则，以启用所需指标 |AuditIfNotExists, 已禁用 |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditMetricAlerts_Audit.json)
