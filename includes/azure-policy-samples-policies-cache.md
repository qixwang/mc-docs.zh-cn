---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 02/13/2020
ms.date: 03/09/2020
ms.author: v-tawe
ms.openlocfilehash: c0a5097c36784eb79785ecb8c15bba7d914d7650
ms.sourcegitcommit: 892137d117bcaf9d88aec0eb7ca756fe39613344
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78048876"
---
|名称 |说明 |效果 |版本 |Source |
|---|---|---|---|
|[只应启用与 Redis 缓存的安全连接](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |审核确认仅启用了通过 SSL 来与 Redis 缓存建立连接。 使用安全连接可确保服务器和服务之间的身份验证并保护传输中的数据免受中间人攻击、窃听攻击和会话劫持等网络层攻击 |Audit, Deny, 已禁用 |1.0.0 |[GitHub](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json)
