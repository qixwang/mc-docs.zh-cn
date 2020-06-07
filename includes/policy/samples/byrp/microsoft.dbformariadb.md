---
author: WenJason
ms.service: azure-policy
ms.topic: include
origin.date: 05/05/2020
ms.date: 06/08/2020
ms.author: v-jay
ms.custom: generated
ms.openlocfilehash: 75eda4ebc743766c1196b6eeb7ceb34918633a48
ms.sourcegitcommit: 9811bf312e0d037cb530eb16c8d85238fd276949
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84275649"
---
|名称 |说明 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[应为 Azure Database for MariaDB 启用异地冗余备份](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0ec47710-77ff-4a3d-9181-6aa50af424d0) |此策略将审核未启用异地冗余备份的任何 Azure Database for MariaDB。 |Audit、Disabled |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMariaDB_Audit.json)。 |
|[MariaDB 服务器应使用虚拟网络服务终结点](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdfbd9a64-6114-48de-a47d-90574dc2e489) |此策略审核未配置为使用虚拟网络服务终结点的 MariaDB 服务器。 有关更多详细信息，请访问 [https://aka.ms/mariadbvirtualnetwork](https://aka.ms/mariadbvirtualnetwork)。 |AuditIfNotExists、Disabled |1.0.1 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_VirtualNetworkServiceEndpoint_Audit.json)。 |
