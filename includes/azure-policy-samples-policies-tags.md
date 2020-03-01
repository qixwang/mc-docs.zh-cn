---
author: rockboyfor
ms.service: azure-policy
ms.topic: include
origin.date: 02/13/2020
ms.date: 03/02/2020
ms.author: v-yeche
ms.openlocfilehash: 273cffeed272ca067ebfe9e37240c76bb1dfda5f
ms.sourcegitcommit: f06e1486873cc993c111056283d04e25d05e324f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "77653281"
---
<!--CHECK THE PORTAL URL ONE BY ONE-->
<!--EXIST URL HAVE BEEN CONFIREM TO ACCESS SUCCESSFULLY-->

|名称 |说明 |效果 |版本 |Source |
|---|---|---|---|
|[追加标记及其默认值](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2a0e14a6-b0a6-4fab-991a-187a4f81c498) |创建或更新任何缺少此标记的资源时追加指定的标记和值。 在更改这些资源之前，请不要修改应用此策略之前创建的资源的标记。 不要应用到资源组。 可以使用新的“modify”效果策略来支持对现有资源中的标记进行修正 (请参阅 https://aka.ms/modifydoc) 。 |append |1.0.0 |[GitHub](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/ApplyTag_Append.json)
|[将标记及其默认值追加到资源组](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F49c88fc8-6fd1-46fd-a676-f12d1d3a4c71) |创建或更新任何缺少此标记的资源组时追加指定的标记和值。 在更改这些资源组之前，请不要修改应用此策略之前创建的资源组的标记。 可以使用新的“modify”效果策略来支持对现有资源中的标记进行修正 (请参阅 https://aka.ms/modifydoc) 。 |append |1.0.0 |[GitHub](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/ResourceGroupApplyTag_Append.json)
|[从资源组追加标记及其值](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9ea02ca2-71db-412d-8b00-7c7ca9fcd32d) |创建或更新任何缺少此标记的资源时，从资源组追加指定的标记及其值。 在更改这些资源之前，请不要修改应用此策略之前创建的资源的标记。 可以使用新的“modify”效果策略来支持对现有资源中的标记进行修正 (请参阅 https://aka.ms/modifydoc) 。 |append |1.0.0 |[GitHub](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/InheritTag_Append.json)
|[需要指定的标记](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F871b6d14-10aa-478d-b590-94f262ecfa99) |强制要求存在某个标记。 不要应用到资源组。 |deny |1.0.0 |[GitHub](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/RequireTag_Deny.json)
|[要求资源组中存在指定的标记](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F96670d01-0a4d-4649-9c89-2d3abc0a5025) |强制要求资源组中存在某个标记。 |deny |1.0.0 |[GitHub](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/ResourceGroupRequireTag_Deny.json)
|[需要标记及其值](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1e30110a-5ceb-460c-a204-c1c3969c6d62) |强制执行所需的标记及其值。 不要应用到资源组。 |deny |1.0.0 |[GitHub](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/RequireTagAndValue_Deny.json)
|[要求资源组中存在标记及其值](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8ce3da23-7156-49e4-b145-24f95f9dcb46) |强制要求资源组中存在所需的标记及其值。 |deny |1.0.0 |[GitHub](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/ResourceGroupRequireTagAndValue_Deny.json)

<!-- Update_Description: new article about azure policy samples policies tags -->
<!--NEW.date: 03/02/2020-->