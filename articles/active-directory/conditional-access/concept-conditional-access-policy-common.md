---
title: 常用条件访问策略 - Azure Active Directory
description: 组织常用的条件访问策略
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/07/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6ae039ce2877d0820159d935aa207803319aae2
ms.sourcegitcommit: 7c80405a6b48380814b4b414e9f8a5756c007880
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067546"
---
# <a name="common-conditional-access-policies"></a>常用条件访问策略

安全默认值很好，但是对于许多组织而言，需要比这些策略提供的更高的灵活性。 例如，许多组织都需要能够从需要多重身份验证的条件访问策略中排除特定帐户，如其紧急访问帐户或不受限管理帐户。 对于这些组织，可以使用本文中提到的常用策略。

![Azure 门户中的条件访问策略](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>紧急访问帐户

有关紧急访问帐户及其重要原因的详细信息，请参阅以下文章： 

* [在 Azure AD 中管理紧急访问帐户](../users-groups-roles/directory-emergency-access.md)

## <a name="typical-policies-deployed-by-organizations"></a>组织部署的典型策略

* [要求对管理员执行 MFA](howto-conditional-access-policy-admin-mfa.md)\*
* [要求将 MFA 用于 Azure 管理](howto-conditional-access-policy-azure-management.md)\*
* [要求对所有用户进行 MFA](howto-conditional-access-policy-all-users-mfa.md)\*
* [阻止旧式身份验证](howto-conditional-access-policy-block-legacy.md)\*
* 基于风险的条件访问（需要 Azure AD Premium P2）
* [要求在进行 MFA 注册时处于受信任的位置](howto-conditional-access-policy-registration.md)
* [按位置阻止访问](howto-conditional-access-policy-location.md)
* [需要兼容设备](howto-conditional-access-policy-compliant-device.md)

## <a name="next-steps"></a>后续步骤

- [使用条件访问 What If 工具模拟登录行为。](troubleshoot-conditional-access-what-if.md)
- [对条件访问使用仅限报告模式来确定新策略决定的影响。](concept-conditional-access-report-only.md)

<!-- Update_Description: wording update -->