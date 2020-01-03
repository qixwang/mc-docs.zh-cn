---
title: Azure MFA 版本和使用计划 - Azure Active Directory
description: 有关多重身份验证客户端以及可用的方法和版本的信息。
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: v-junlch
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1aa1005b094f6f4899955cccb1f0e78777f85e0b
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335134"
---
# <a name="how-to-get-azure-multi-factor-authentication"></a>如何获取 Azure 多重身份验证

如果想要保护帐户，应该在组织中标配双重验证。 对资源拥有访问特权的帐户尤其需要此功能。 为此，Microsoft 为 Office 365 和 Azure Active Directory (Azure AD) 管理员提供了基本的双重验证功能，无需额外费用。 如果想要升级管理员的功能，或者将双重验证扩展到其他用户，可以用多种方式购买 Azure 多重身份验证。

> [!IMPORTANT]
> 本文旨在指导用户如何以不同的方式购买 Azure 多重身份验证。 有关定价和计费的具体详细信息，请始终参阅[多重身份验证定价页](https://www.azure.cn/pricing/details/multi-factor-authentication/)。
>

## <a name="available-versions-of-azure-multi-factor-authentication"></a>可用的 Azure 多重身份验证版本

下表介绍了多重身份验证的版本之间的差别：

| 版本 | 说明 |
| --- | --- |
| 免费选项 | 利用 Azure AD 免费权益的客户可以使用 `security defaults` 在他们的环境中启用多重身份验证。 |
| 适用于 Office 365 的多重身份验证 | 此版本通过 Office 365 或 Microsoft 365 门户进行管理。 管理员可以[使用双重验证来保护 Office 365 资源](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6)。 此版本是 Office 365 订阅的一部分。 |
| 面向 Azure AD 管理员的多重身份验证 | Azure AD 租户中被分配了 Azure AD 全局管理员角色的用户可以免费启用双重验证。 |
| Azure 多重身份验证 | Azure 多重身份验证（通常称为“完整”版本）提供了最丰富的功能集。 它通过 [Azure 门户](https://portal.azure.cn)、高级报告及支持一系列本地和云应用程序来提供其他配置选项。 Azure 多重身份验证是 [Azure Active Directory Premium](https://www.microsoft.com/cloud-platform/azure-active-directory-features) 和 [Microsoft 365 商业版](https://www.microsoft.com/microsoft-365/business)的一项功能。 |

> [!NOTE]
> 自 2018 年 9 月 1 日起，新客户无法再将 Azure 多重身份验证作为独立产品进行购买。 多重身份验证将继续可以作为 Azure AD Premium 或 Microsoft 365 商业版许可证中的功能使用。

## <a name="feature-comparison-of-versions"></a>版本功能比较

下表提供了 Azure 多重身份验证的各个版本中可用的功能列表。

> [!NOTE]
> 此比较表讨论了每个版本的多重身份验证的部分功能。 如果拥有完整的 Azure 多重身份验证服务，某些功能可能不可用，具体取决于是否[在云中使用 MFA](concept-mfa-howitworks.md)。
>

| 功能 | 适用于 Office 365 的多重身份验证 | 面向 Azure AD 管理员的多重身份验证 | Azure 多重身份验证 | 安全默认值 | 
| --- |:---:|:---:|:---:|:---:|
| 使用 MFA 保护 Azure AD 管理员帐户 |● |●（仅适用于 Azure AD 全局管理员帐户） |● |● |
| 将移动应用用作第二个因素 |● |● |● |● |
| 将电话呼叫用作第二个因素 |● |● |● |   |
| 将短信用作第二个因素 |● |● |● |   |
| 管理员控制验证方法 |● |● |● |   |
| 使用 MFA 保护非管理员帐户 |● | |● |● |
| PIN 模式 | | |● |   |
| 欺诈警报 | | |● |   |
| 一次性跳过 | | |● |   |
| 通话的自定义问候语 | | |● |   |
| 通话的自定义呼叫方 ID | | |● |   |
| 受信任的 IP | | |● |   |
| 记住受信任的设备的 MFA |● |● |● |   |

> [!IMPORTANT]
> 从 2019 年 3 月开始，免费/试用 Azure AD 租户中的 MFA 和 SSPR 用户将无法使用电话呼叫选项。 SMS 消息不受此更改影响。 付费 Azure AD 租户中的用户将继续可以使用电话呼叫。 此更改仅影响免费/试用 Azure AD 租户。

## <a name="how-to-turn-on-azure-multi-factor-authentication-for-azure-ad-administrators"></a>如何为 Azure AD 管理员启用 Azure 多重身份验证

Azure AD 租户中被分配了全局管理员角色的用户可以免费为其 Azure AD 全局管理员帐户启用双重验证。 如果使用的是 Microsoft 帐户，则可以使用 Microsoft 帐户支持文章[关于双重验证](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification)中的指南注册多重身份验证。 如果未使用 Microsoft 帐户，请使用文章[如何对用户或组要求双重验证](howto-mfa-userstates.md)中的指南为全局管理员启用多重身份验证。

## <a name="next-steps"></a>后续步骤

- 有关定价详细信息，请参阅 [Azure MFA 定价](https://www.azure.cn/pricing/details/multi-factor-authentication/)。

<!-- Update_Description: wording update -->
