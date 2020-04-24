---
title: Azure 多重身份验证常见问题解答 - Azure Active Directory
description: 与 Azure 多重身份验证相关的常见问题与解答。
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: troubleshooting
ms.date: 12/11/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b8fce6bd9f2b370e82be04ea1af1349d0fc4816
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75334920"
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>有关 Azure 多重身份验证的常见问题

本“常见问题解答”文章解答有关 Azure 多重身份验证和使用多重身份验证服务的常见问题。 其中的问题已划分为常规服务问题、计费模式问题、用户体验问题和故障排除问题。

## <a name="general"></a>常规

## <a name="billing"></a>计费
可参阅 [多重身份验证定价页](https://www.azure.cn/pricing/details/multi-factor-authentication/)获得大多数计费问题的答案。

**问：通过电话或短信进行身份验证时，我的组织是否需要付费？**

否。对于通过 Azure 多重身份验证拨打的每个电话或者向用户发送的每条短信，都不需要付费。

用户可能需要为收到的电话或短信支付费用，取决于个人电话服务。


## <a name="manage-and-support-user-accounts"></a>管理和支持用户帐户

**问：如果用户的手机未收到响应，我该告诉他们怎么做？**

让用户在 5 分钟内尝试最多 5 次，以便收到电话或短信进行身份验证。 Microsoft 使用多个提供程序，用于进行呼叫和发送短信。 如果这不起作用，请使用 Microsoft 打开支持案例以进一步排除故障。

如果上述步骤不起作用，但愿用户已配置多种验证方法。 请告诉他们再次尝试登录，但需要在登录页上选择另一种验证方法。

可以让用户转到[最终用户故障排除指南](../user-help/multi-factor-authentication-end-user-troubleshoot.md)。

**问：如果某个用户无法进入其帐户，我该办什么？**

可以要求用户再次完成注册过程来重置其帐户。 详细了解[管理云中 Azure 多重身份验证的用户和设备设置](howto-mfa-userdevicesettings.md)。

**问：我的用户说，有时他们收不到短信，或者验证超时。**

发送短信无法得到保障，因为存在可能影响服务可靠性的不可控因素。 这些因素包括目标国家/地区、移动电话运营商和信号强度。

如果用户经常无法可靠地接收短信，请告诉他们改用移动应用或电话验证方法。 移动应用可以同时通过手机网络和 Wi-Fi 连接接收通知。 此外，即使设备根本没有信号，也可以生成验证码。 Microsoft 验证器应用适用于 [Android](https://go.microsoft.com/fwlink/?Linkid=825072)、[iOS](https://go.microsoft.com/fwlink/?Linkid=825073) 和 [Windows Phone](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6)。


**问：为何系统提示用户注册其安全信息？**
有多种原因会导致系统提示用户注册其安全信息：

- 该用户的管理员已在 Azure AD 中为其启用 MFA，但没有为其帐户注册安全信息。
- 该用户已在 Azure AD 中启用自助密码重置。 以后如果用户忘记了密码，安全信息可帮助他们重置密码。
- 该用户访问的应用程序配置了一个要求使用 MFA 的条件访问策略，但该应用程序以前未注册 MFA。
- 该用户正在将某个设备注册到 Azure AD（包括 Azure AD Join），并且组织要求使用 MFA 进行设备注册，但该用户以前未注册 MFA。
- 该用户正在 Windows 10 中生成 Windows Hello for Business（需要 MFA），但以前未注册 MFA。
- 组织已创建并启用一个 MFA 注册策略，该策略已应用到该用户。
- 该用户以前已注册 MFA，但选择的验证方法后来被管理员禁用。 因此，该用户必须再次完成 MFA 注册，以选择新的默认验证方法。

## <a name="errors"></a>错误

**问：如果用户使用移动应用通知时看到“身份验证请求不适用于已激活的帐户”错误消息，该怎么办？**

告诉他们按照此过程从移动应用中删除其帐户，并重新添加：

1. 转到 Azure 门户配置文件，并使用组织帐户登录。
2. 选择“其他安全性验证”  。
3. 从移动应用中删除现有帐户。
4. 单击“配置”  ，并按照说明重新配置移动应用。

**问：如果用户在登录非浏览器应用程序时看到 0x800434D4L 错误消息，该怎么办？**

尝试登录在本地计算机上安装的非浏览器应用程序，且此应用程序无法使用需要双重验证的帐户时，将发生 0x800434D4L 错误。

此错误的解决方法是，使用不同的用户帐户执行管理员相关操作和非管理员操作。 稍后，可以在管理员帐户与非管理员帐户之间链接邮箱，以便能够使用非管理员帐户登录到 Outlook。 若要详细了解此解决方案，请了解如何[让管理员能够打开和查看用户邮箱的内容](https://help.outlook.com/141/gg709759.aspx?sl=1)。

## <a name="next-steps"></a>后续步骤

如果此处未解答问题，请将其留在页面底部的评论中。 或者，以下是一些用于获取帮助的其他选项：

* 在 [Microsoft 支持知识库](https://support.microsoft.com) 中搜索常见技术问题的解决方法。
* 在 [Azure Active Directory 论坛](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)中搜索和浏览来自社区的技术问题与解答，或者提出自己的问题。
* 通过 [Azure 多重身份验证服务器 (PhoneFactor) 支持](https://support.microsoft.com/oas/default.aspx?prid=14947)联系支持专业人员。 与我们联系时，尽可能包含有关问题的更多信息将很有帮助。 可以提供的信息包括出现错误的页面、特定错误代码、特定会话 ID 以及看到错误的用户的 ID。

<!-- Update_Description: wording update -->
