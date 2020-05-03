---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 04/03/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: f4318d2596cab5cb145b72b4e7c63bf51a6d9147
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82595259"
---
|名称 |说明 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[应为虚拟机启用 Azure 备份](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F013e242c-8828-4970-87b3-ab247555486d) |此策略可帮助审核是否为所有虚拟机启用了 Azure 备份服务。 Azure 备份是一个经济高效的一键式备份解决方案，可简化数据恢复，并且比其他云备份服务更易于启用。 |AuditIfNotExists、Disabled |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachines_EnableAzureBackup_Audit.json)。

<!-- |[Configure backup on VMs of a location to an existing central Vault in the same location](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F09ce66bc-1220-4153-8104-e3f51c936913) |This policy configures Azure Backup protection on VMs in a given location to an existing central vault in the same location. It applies to only those VMs that are not already configured for backup. It is recommended that this policy is assigned to not more than 200 VMs. If the policy is assigned for more than 200 VMs, it can result in the backup getting triggered a few hours beyond the defined schedule. This policy will be enhanced to support more VM images. |deployIfNotExists, auditIfNotExists, disabled |1.0.0 |[Link](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachineBackup_Backup_DeployIfNotExists.json) -->
