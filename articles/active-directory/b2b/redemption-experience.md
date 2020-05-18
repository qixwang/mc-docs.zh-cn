---
title: B2B 协作中的邀请兑换 - Azure AD
description: 介绍最终用户的 Azure AD B2B 协作邀请兑换体验，包括对隐私条款的同意。
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: v-junlch
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8567c56b63c9d144b11fe4c43e8bfc18e331981
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82515647"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B 协作邀请兑换

本文介绍来宾用户如何访问你的资源及其遇到的同意过程。 如果你向来宾发送邀请电子邮件，邀请中会包含一个链接，来宾可以通过该链接兑换访问应用或门户的权限。 邀请电子邮件只是供来宾获取资源访问权限的方式之一。 替代方法是将来宾添加到目录，并为其提供指向门户或你想要共享的应用的直接链接。 无论来宾使用哪种方法，系统都会引导他们完成首次同意过程。

将来宾用户添加到目录时，来宾用户帐户的同意状态（可在 PowerShell 中查看）最初设置为 PendingAcceptance。 在来宾接受邀请并同意你的隐私策略和使用条款之前，会一直保留此设置。 此后，同意状态会更改为“已接受”，且不再向来宾显示同意页。

## <a name="redemption-through-the-invitation-email"></a>通过邀请电子邮件兑换

使用 Azure 门户将来宾用户添加到目录的过程中，会向来宾发送邀请电子邮件。 在[使用 PowerShell](/active-directory/b2b/b2b-quickstart-invite-powershell) 将来宾用户添加到目录时，也可选择发送邀请电子邮件。 下面是来宾按照电子邮件中的链接进行兑换时的体验说明。

1. 来宾接收 Microsoft Invitations 发送的[邀请电子邮件](/active-directory/b2b/invitation-email-elements)。
2. 来宾选择电子邮件中的“开始使用”。
4. 来宾根据下面所述完成[同意体验](#consent-experience-for-the-guest)。

## <a name="redemption-through-a-direct-link"></a>通过直接链接兑换

可以向来宾提供应用或门户的直接链接，作为邀请电子邮件的替代方法。 首先需要通过 Azure 门户或 [PowerShell](/active-directory/b2b/b2b-quickstart-invite-powershell) 将来宾用户添加到目录。 当来宾使用直接链接而不是邀请电子邮件时，系统仍会引导他们完成首次同意体验。

> [!IMPORTANT]
> 直接链接必须特定于租户。 换言之，它必须包含租户 ID 或已验证的域，以便能够在共享应用所在的租户中对来宾进行身份验证。 常见的 URL（如 https://account.activedirectory.windowsazure.cn/r#/applications ）对来宾不起作用，因为它会重定向到来宾的主租户进行身份验证。 下面是使用租户上下文的直接链接的一些示例：
 > - 应用访问面板： https://account.activedirectory.windowsazure.cn/r#/applications/?tenantid=&lt ;租户 ID&gt; 
 > - 验证的域的应用访问面板： https://account.activedirectory.windowsazure.cn/r#/applications/&lt ;验证的域&gt;
 > - Azure 门户： https://portal.azure.cn/&lt ;租户 ID&gt;

在某些情况下，建议使用邀请电子邮件而不要使用直接链接。 如果这些特殊情况对贵组织而言很重要，建议使用仍会发送邀请电子邮件的方法来邀请用户：
 - 有时，由于与联系人对象（例如，Outlook 联系人对象）存在冲突，受邀用户对象可能会没有电子邮件地址。 在这种情况下，用户必须单击邀请电子邮件中的兑换 URL。
 - 用户可使用受邀电子邮件地址的别名登录。 （别名指与电子邮件帐户关联的其他电子邮件地址。）在这种情况下，用户必须单击邀请电子邮件中的兑换 URL。

## <a name="consent-experience-for-the-guest"></a>来宾的同意体验

当来宾首次登录访问合作伙伴组织中的资源时，系统会引导他们在以下页中完成操作。 

1. 来宾查看“查看权限”页，其中描述了邀请组织的隐私声明。 用户若要继续操作，必须接受允许他人根据邀请组织的隐私策略使用用户信息这一条件。

   ![显示“查看权限”页的屏幕截图](./media/redemption-experience/review-permissions.png) 

   > [!NOTE]
   > 有关如何以租户管理员身份链接到组织隐私声明的信息，请参阅[如何：在 Azure Active Directory 中添加组织的隐私信息](/active-directory/fundamentals/active-directory-properties-area)。

2. 如果配置了使用条款，则来宾需打开并查看使用条款，然后选择“接受”。 

   ![显示新使用条款的屏幕截图](./media/redemption-experience/terms-of-use-accept.png) 

3. 除非另有指定，否则来宾会被重定向到应用访问面板，其中列出了来宾可以访问的应用程序。

   ![显示应用访问面板的屏幕截图](./media/redemption-experience/myapps.png) 

在目录中，来宾的“邀请已接受”值将更改为“是” 。 

## <a name="next-steps"></a>后续步骤

- [什么是 Azure AD B2B 协作？](what-is-b2b.md)
- [在 Azure 门户中添加 Azure Active Directory B2B 协作用户](add-users-administrator.md)
- [使用 PowerShell 添加 Azure Active Directory B2B 协作用户](customize-invitation-api.md#powershell)
- [以来宾用户身份离开组织](leave-the-organization.md)

