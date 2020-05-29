---
title: 使用自定义策略配置会话行为 - Azure Active Directory B2C | Microsoft Docs
description: 在 Azure Active Directory B2C 中使用自定义策略配置会话行为。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/18/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 0eca98b1b000c61f85a8b57273d7eb55d79f518b
ms.sourcegitcommit: 87e789550ea49ff77c7f19bc68fad228009fcf44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83749523"
---
# <a name="configure-session-behavior-using-custom-policies-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中使用自定义策略配置会话行为

使用 Azure Active Directory B2C (Azure AD B2C) 中的[单一登录 (SSO) 会话](session-overview.md)管理，管理员可在用户已通过身份验证之后控制与用户的交互。 例如，管理员可以控制是否显示标识提供者选择，或是否需要再次输入帐户详细信息。 本文介绍如何配置 Azure AD B2C SSO 的设置。

## <a name="session-behavior-properties"></a>会话行为属性

可使用以下属性来管理 Web 应用程序会话：

- **Web 应用会话生存期（分钟）** - 身份验证成功后，存储在用户浏览器上的 Azure AD B2C 会话 Cookie 的生存期。
    - 默认值 = 86400 秒（1440 分钟）。
    - 最小值（含）= 900 秒（15 分钟）。
    - 最大值（含）= 86400 秒（1440 分钟）。
- **Web 应用会话超时** - [会话到期类型](session-overview.md#session-expiry-type)：“滚动”或“绝对” 。 
- **单一登录配置** - Azure AD B2C 租户中跨多个应用和用户流的单一登录 (SSO) 行为的[会话范围](session-overview.md#session-scope)。 

## <a name="configure-the-properties"></a>配置属性

若要更改会话行为和 SSO 配置，需要在 [RelyingParty](relyingparty.md) 元素内添加 **UserJourneyBehaviors** 元素。  **UserJourneyBehaviors** 元素必须紧跟在 **DefaultUserJourney** 之后。 UserJourneyBehavors 元素应当如以下示例所示：

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

## <a name="next-steps"></a>后续步骤

- 详细了解 [Azure AD B2C 会话](session-overview.md)。

