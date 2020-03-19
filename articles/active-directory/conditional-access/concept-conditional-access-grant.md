---
title: 条件访问策略中的授权控制 - Azure Active Directory
description: Azure AD 条件访问策略中的授权控制是什么
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: a2917f07003a4a359765ba2d668af55043ae3af0
ms.sourcegitcommit: 4ba6d7c8bed5398f37eb37cf5e2acafcdcc28791
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79134170"
---
# <a name="conditional-access-grant"></a>条件访问：授予

在条件访问策略中，管理员可以利用访问控制来授予或阻止对资源的访问权限。

![包含授权控制且需要多重身份验证的条件访问策略](./media/concept-conditional-access-grant/conditional-access-grant.png)

## <a name="block-access"></a>阻止访问

阻止操作会考虑到任何分配，根据条件访问策略配置阻止访问。

阻止是一种强有力的控制，运用此项控制时，应具备相应的知识。 在启用该控制之前，管理员应使用“仅限报告”模式进行测试。

## <a name="grant-access"></a>授予访问权限

在授予访问权限时，管理员可以选择强制实施一项或多项控制。 这些控制包括以下选项： 

- [需要多重身份验证（Azure 多重身份验证）](../authentication/concept-mfa-howitworks.md)
- [要求将设备标记为合规 (Microsoft Intune)](https://docs.microsoft.com/intune/protect/device-compliance-get-started)
- [“需要已批准的客户端应用”](app-based-conditional-access.md)

当管理员组合使用这些选项时，可以选择以下方法：

- 需要所有选定控制（控制**和**控制）
- 需要某一选定控制（控制**或**控制）

默认情况下，条件访问需要所有选定控制。

### <a name="require-multi-factor-authentication"></a>需要多重身份验证

选中此复选框会要求用户执行 Azure 多重身份验证。 在[规划基于云的 Azure 多重身份验证部署](../authentication/howto-mfa-getstarted.md)一文中可以找到有关部署 Azure 多重身份验证的详细信息。

### <a name="require-device-to-be-marked-as-compliant"></a>要求将设备标记为合规

部署了 Microsoft Intune 的组织可以根据从其设备返回的信息来识别符合具体合规要求的设备。 此策略合规性信息将从 Intune 转发到 Azure AD，其中的条件访问可以决定是要授予还是阻止对资源的访问。 有关合规性策略的详细信息，请参阅[使用 Intune 在设备上设置规则以允许访问组织中的资源](https://docs.microsoft.com/intune/protect/device-compliance-get-started)一文。

### <a name="require-approved-client-app"></a>需要批准的客户端应用

组织可以要求只能尝试从已批准的客户端应用访问选定的云应用。

此设置适用于以下客户端应用：

- Azure 信息保护
- Microsoft Bookings
- Microsoft Cortana
- Microsoft Dynamics 365
- Microsoft Edge
- Microsoft Excel
- Microsoft Flow
- Microsoft Intune Managed Browser
- Microsoft Invoicing
- Microsoft Kaizala
- Microsoft Launcher
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerApps
- Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype for Business
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- 微软待办
- Microsoft Visio
- Microsoft Word
- Microsoft Yammer

**备注**

- 批准的客户端应用支持 Intune 移动应用管理功能。
- “需要批准的客户端应用”  要求：
   - 仅支持 iOS 和 Android 作为设备平台条件。
- 条件访问无法将 InPrivate 模式下的 Microsoft Edge 视为已批准的客户端应用。

### <a name="require-app-protection-policy"></a>需要应用保护策略

在条件访问策略中，你可以要求客户端应用上存在 Intune 应用保护策略，然后才能访问所选云应用。 

此设置适用于以下客户端应用：

- Microsoft Cortana
- Microsoft OneDrive
- Microsoft Outlook
- Microsoft Planner

**备注**

- 适用于应用保护策略的应用支持带策略保护的 Intune 移动应用程序管理功能。
- “需要应用保护策略”要求： 
    - 仅支持 iOS 和 Android 作为设备平台条件。

## <a name="next-steps"></a>后续步骤

- [条件访问：会话控制](concept-conditional-access-session.md)

- [条件访问常见策略](concept-conditional-access-policy-common.md)

