---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 02/13/2020
ms.date: 03/09/2020
ms.author: v-tawe
ms.openlocfilehash: 7b91998055676828d152c0e71584894a49b5b5a5
ms.sourcegitcommit: 892137d117bcaf9d88aec0eb7ca756fe39613344
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78048901"
---
|名称 |说明 |效果 |版本 |Source |
|---|---|---|---|
|[应启用搜索服务的诊断日志](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb4330a05-a843-4bc8-bf9a-cacce50c67f4) |审核是否已启用诊断日志。 这样便可以在发生安全事件或网络受到威胁时重新创建活动线索以用于调查目的 |AuditIfNotExists, 已禁用 |2.0.0 |[GitHub](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Search/Search_AuditDiagnosticLog_Audit.json)
