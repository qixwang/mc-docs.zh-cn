---
title: Azure Active Directory 条件访问中的条件是什么？ | Microsoft Docs
description: 了解如何在 Azure Active Directory 条件访问中使用条件来触发策略。
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 01/09/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2660487833468254b0fd08b87b48b676e938af23
ms.sourcegitcommit: bc5f8b4f8ccd7c723f64055825508d1dfcc2162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75859323"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Azure Active Directory 条件访问中的条件是什么？

使用 [Azure Active Directory (Azure AD) 条件访问](overview.md)，可以控制用户访问云应用的方式。 在条件访问策略中，定义触发策略的诱因（“出现这种情况时”）的响应（“执行此操作”）。

![原因和响应](./media/conditions/10.png)

在条件访问的上下文中，“出现这种情况时”称为“条件”。   “则执行此操作”称为“访问控制”。   条件与访问控制的组合表示一种条件访问策略。

![条件访问策略](./media/conditions/61.png)

不会应用未在条件访问策略中配置的条件。 要将条件访问策略应用到环境，某些条件是[必需的](best-practices.md)。

本文概述条件，以及如何在条件访问策略中使用条件。 

## <a name="users-and-groups"></a>用户和组

用户和组条件在条件访问策略中是必需的。 在策略中，可以选择**所有用户**或选择特定的用户和组。

![用户和组](./media/conditions/111.png)

选择“所有用户”时，策略将应用到目录中的所有用户，包括来宾用户。 

**选择用户和组**时，可以设置以下选项：

* “目录角色”基于用户的角色分配定位策略。  此条件支持目录角色，如“全局管理员”  或“密码管理员”  。
* “用户和组”面向特定的用户集。  例如，将某个人力资源应用选作云应用时，可以选择包含人力资源部所有成员的组。 某个组可以是 Azure AD 中任何类型的组，包括动态组，或分配的安全组和通讯组。

此外，你还可以从策略中排除特定的用户或组。 例如，在策略强制实施多重身份验证 (MFA) 的情况下，往往会排除服务帐户。

若要部署新策略，面向特定的用户集十分有用。 在新策略中，应该只将初始用户集用作目标来验证策略行为。

## <a name="cloud-apps-and-actions"></a>云应用和操作

云应用是受 Azure AD 应用程序代理保护的网站、服务或终结点。 有关受支持云应用的详细说明，请参阅[云应用分配](technical-reference.md#cloud-apps-assignments)。 “云应用或操作”条件在条件访问策略中是必需的。  在策略中，可以选择“所有云应用”  ，或者通过“选择应用”来指定应用。 

组织可从以下选项中选择：

* “所有云应用”  ，前提是将基线策略应用到整个组织。 对于要求执行多重身份验证（由于检测到任何云应用存在登录风险）的策略，请使用此选项。 应用到“所有云应用”的策略将应用于对所有网站和服务的访问权限。 此设置并非仅限用于“选择应用”列表上显示的云应用。
* “选择应用”，以便根据策略将特定服务指定为目标  。 例如，可以要求用户使用合规的设备访问 SharePoint Online。 当其他服务访问 SharePoint 内容时，也会对这些服务应用此策略。 例如 Microsoft Teams。

> [!NOTE]
> 可以从策略中排除特定的应用。 但是，这些应用仍受到应用于它们访问的服务的策略的限制。

“用户操作”是可供用户执行的任务。  目前唯一支持的操作是“注册安全信息(预览版)”  。当启用了组合注册功能的用户尝试注册其安全信息时，此操作允许条件访问策略来强制实施。 


## <a name="device-platforms"></a>设备平台

设备平台根据设备上运行的操作系统来定义特征。 Azure AD 使用设备（例如用户代理）提供的信息来标识平台。 此信息未经验证。 我们建议在所有平台中应用某个策略。 该策略应阻止访问、要求符合 Microsoft Intune 政策，或要求设备加入域。 默认设置是将策略应用到所有设备平台。 

![配置设备平台](./media/conditions/24.png)

有关支持的设备平台列表，请参阅[设备平台条件](technical-reference.md#device-platform-condition)。

有关包含设备平台条件的其他方案，请参阅 [Azure Active Directory 基于应用的条件访问](app-based-conditional-access.md)。

## <a name="device-state"></a>设备状态

设备状态条件从条件访问策略中排除已加入混合 Azure AD 的设备，以及标记为合规的设备。 

![配置设备状态](./media/conditions/112.png)

当策略仅应用到非托管设备以使会话更安全时，此条件非常有用。 例如，当设备处于非托管状态时，仅强制执行 Microsoft Cloud App Security 会话控制。 

## <a name="locations"></a>位置

使用位置可以基于尝试连接的位置定义条件。 

![配置位置](./media/conditions/25.png)

此条件的常见用例包括提供以下保护的策略：

- 要求用户在企业网络外部访问服务时执行多重身份验证。  
- 阻止特定国家或地区的用户访问服务。 

有关详细信息，请参阅 [Azure Active Directory 条件访问中的位置条件是什么？](/active-directory/conditional-access/location-condition)。

## <a name="client-apps"></a>客户端应用

默认情况下，条件访问策略适用于以下应用：

- **[浏览器应用](technical-reference.md#supported-browsers)** -  浏览器应用包括使用 SAML、WS-Federation 或 OpenID Connect Web SSO 协议的网站。 这也适用于已注册为 OAuth 机密客户端的任何网站或 Web 服务。 例如，Office 365 SharePoint 网站。 
- **[使用新式身份验证的移动和桌面应用](technical-reference.md#supported-mobile-applications-and-desktop-clients)** - 这些应用包括 Office 桌面应用和手机应用。 


另外，可以让策略针对不使用新式身份验证的特定客户端应用，例如：

- **[Exchange ActiveSync 客户端](conditions.md#exchange-activesync-clients)** - 当某项策略阻止使用 Exchange ActiveSync 时，受影响的用户会收到一封有关隔离的电子邮件，说明为何会阻止他们。 必要时，该电子邮件会说明如何将设备注册到 Intune。
- **[其他客户端](block-legacy-authentication.md)** - 这些应用包括通过邮件协议（如 IMAP、MAPI、POP、SMTP）使用基本身份验证的客户端，以及不使用新式身份验证的旧版 Office 应用。 有关详细信息，请参阅[如何对 Office 2013 和 Office 2016 客户端应用使用新式身份验证](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016)。

![客户端应用](./media/conditions/41.png)

此条件的常见用例包括提供以下要求的策略：

- **[阻止向 Azure AD 进行的旧身份验证](block-legacy-authentication.md)** （其他客户端）
- 阻止从 Web 应用程序访问，但允许从移动和桌面应用程序访问。

### <a name="exchange-activesync-clients"></a>Exchange ActiveSync 客户端

只有在以下情况下，才可以选在“Exchange ActiveSync 客户端”： 

- Microsoft Office 365 Exchange Online 是所选的唯一云应用。

    ![云应用](./media/conditions/32.png)

- 未在策略中配置其他条件。 但是，可以缩小此条件的范围，使之仅应用到[支持的平台](technical-reference.md#device-platform-condition)。
 
    ![仅将策略应用到支持的平台](./media/conditions/33.png)

如果系统要求使用经审核的应用，则受影响的用户会收到 Outlook 移动客户端的安装和使用指南。

在其他情况下（例如，需要 MFA），则会阻止受影响用户，因为使用基本身份验证的客户端不支持 MFA。

只能将此设置用于用户和组。 它不支持来宾或角色。 如果配置了来宾或角色条件，则会阻止所有用户，因为条件访问无法确定是否应将策略应用于用户。

有关详细信息，请参阅：

- [为 Azure Active Directory 条件访问设置 SharePoint Online 和 Exchange Online](/active-directory/conditional-access/conditional-access-for-exo-and-spo)。
- [基于 Azure Active Directory 应用的条件访问](/active-directory/conditional-access/app-based-conditional-access)。 

## <a name="next-steps"></a>后续步骤

- 若要了解如何配置条件访问策略，请参阅[快速入门：使用 Azure Active Directory 条件访问要求针对特定应用进行 MFA](app-based-mfa.md)。
- 若要为环境配置条件访问策略，请参阅 [Azure Active Directory 中条件访问的最佳做法](best-practices.md)。 

