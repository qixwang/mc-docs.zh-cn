---
title: 条件访问策略中的云应用或操作 - Azure Active Directory
description: 什么是 Azure AD 条件访问策略中的云应用或操作
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b084b104ed6adeac25adc82cc0789e8ee58da3f3
ms.sourcegitcommit: f06e1486873cc993c111056283d04e25d05e324f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "77653340"
---
# <a name="conditional-access-cloud-apps-or-actions"></a>条件访问：云应用或操作

云应用或操作是条件访问策略中的关键信号。 使用条件访问策略，管理员可以分配对特定应用程序或操作的控制。

- 管理员可以从包含内置 Microsoft 应用程序和任何 Azure AD 集成应用程序的应用程序列表中进行选择。
- 管理员可以选择不基于云应用程序但基于用户操作来定义策略。 唯一受支持的操作是注册安全信息（预览版），允许条件访问围绕组合的安全信息注册体验来强制实施控制。

![定义条件访问策略并指定云应用](./media/concept-conditional-access-cloud-apps/conditional-access-cloud-apps-or-actions.png)

## <a name="azure-cloud-applications"></a>Azure 云应用程序

许多现有的 Azure 云应用程序都包含在你可以从中进行选择的应用程序列表中。 

管理员可以为来自 Microsoft 的以下云应用分配条件访问策略。 某些应用（例如 Office 365（预览版）和 Azure 管理）包含多个相关的子应用或服务。 以下列表不完整，并且可能会发生更改。

- [Office 365（预览版）](#office-365-preview)
- Azure Analysis Services
- Azure DevOps
- Azure SQL 数据库和数据仓库
- Dynamics CRM Online
- [Azure 管理](#microsoft-azure-management)
- Azure 订阅管理
- Microsoft Cloud App Security
- Microsoft Commerce Tools 访问控制门户
- Microsoft Commerce Tools 身份验证服务
- Microsoft Flow
- Microsoft Forms
- Microsoft Intune
- Microsoft Intune 注册 
- Microsoft Planner
- Microsoft PowerApps
- 必应中的 Microsoft 搜索
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- Office 365 Exchange Online
- Office 365 SharePoint Online
- Office 365 Yammer
- Office Delve
- Office Sway
- Outlook 组
- Power BI 服务
- Project Online
- Skype for Business Online
- 虚拟专用网络 (VPN)
- Windows Defender ATP

### <a name="office-365-preview"></a>Office 365（预览版）

Office 365 提供基于云的高效生产和协作服务，如 Exchange、SharePoint 和 Microsoft Teams。 Office 365 云服务已深度集成，以确保用户拥有顺畅的协作体验。 在创建策略时，此集成可能会导致混淆，因为某些应用（例如 Microsoft Teams）依赖于其他应用，例如 SharePoint 或 Exchange。

使用 Office 365（预览版）应用可以一次同时将这些服务作为目标。 建议使用新的 Office 365（预览版）应用，而不是以个别云应用作为目标。 以此组应用程序作为目标有助于避免因策略和依赖关系不一致而导致的问题。

如果需要，管理员可以选择从策略中排除特定应用，方法是在策略中包括 Office 365（预览版）应用并排除他们选择的特定应用。

Office 365（预览版）客户端应用中包含的关键应用程序：

   - Microsoft Flow
   - Microsoft Forms
   - Microsoft Stream
   - 微软待办
   - Microsoft Teams
   - Office 365 Exchange Online
   - Office 365 SharePoint Online
   - Office 365 搜索服务
   - Office 365 Yammer
   - Office Delve
   - Office Online
   - Office.com
   - OneDrive
   - PowerApps
   - Skype for Business Online
   - Sway

### <a name="azure-management"></a>Azure 管理

Azure 管理应用程序包括多个基础服务。 

   - Azure 门户
   - Azure 资源管理器提供程序
   - 经典部署模型 API
   - Azure PowerShell
   - Visual Studio 订阅管理员门户
   - Azure DevOps
   - Azure 数据工厂门户

> [!NOTE]
> Azure 管理应用程序适用于调用 Azure 资源管理器 API 的 Azure PowerShell。 它不适用于调用 Microsoft Graph 的 Azure AD PowerShell。

## <a name="user-actions"></a>用户操作

用户操作是可由用户执行的任务。 目前唯一支持的操作是“注册安全信息(预览版)”  。当启用了组合注册功能的用户尝试注册其安全信息时，此操作允许条件访问策略来强制实施。 

## <a name="next-steps"></a>后续步骤

- [条件访问：条件](concept-conditional-access-conditions.md)

- [条件访问常见策略](concept-conditional-access-policy-common.md)
- [客户端应用程序依赖项](service-dependencies.md)

<!-- Update_Description: wording update -->