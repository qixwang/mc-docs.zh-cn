---
title: 云中的 Azure MFA 入门
description: Azure 多重身份验证和条件访问入门
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
origin.date: 09/01/2018
ms.date: 10/25/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 56eb6b68071d44e0b2dded9a02e318bf321ac729
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72912733"
---
# <a name="deploy-cloud-based-azure-multi-factor-authentication"></a>部署基于云的 Azure 多重身份验证

Azure 多重身份验证 (Azure MFA) 入门是一个直截了当的过程。

在开始之前，请确保满足以下先决条件：

- Azure AD 租户中的全局管理员帐户。 
- 分配给用户的正确许可证。 如需详细信息，请参阅文章[如何获取 Azure 多重身份验证](concept-mfa-licensing.md)。

## <a name="choose-how-to-enable"></a>选择启用方法

通过更改用户状态启用 - 这是需要进行双重验证的传统方法。  它与云中的 Azure MFA 配合工作。 使用此方法要求用户**每次**登录时都执行双重验证。 可在[如何要求对用户进行双重验证](howto-mfa-userstates.md)中找到有关此方法的详细信息。

> [!Note]
> 有关许可和定价的详细信息，请参见 [Azure AD](https://www.azure.cn/pricing/details/active-directory/
) 和[多重身份验证](https://www.azure.cn/pricing/details/multi-factor-authentication/)定价页。

## <a name="choose-authentication-methods"></a>选择身份验证方法

根据组织的要求至少为用户启用一种身份验证方法。 我们发现，如果为用户启用了身份验证，则 Microsoft Authenticator 应用可提供最佳用户体验。


## <a name="enable-multi-factor-authentication-with-conditional-access"></a>结合条件访问启用多重身份验证

使用全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

### <a name="choose-verification-options"></a>选择验证选项

在启用 Azure 多重身份验证之前，组织必须确定允许的验证选项。 在本练习中，我们将启用电话呼叫和手机短信身份验证方法，因为这是大多数人都可以使用的常规选项。 

1. 浏览至“Azure Active Directory”  、“用户”  、“多重身份验证”  。

   ![从 Azure 门户中的“Azure AD 用户”边栏选项卡访问“多重身份验证”门户](./media/howto-mfa-getstarted/users-mfa.png)

1. 在打开的新选项卡中，浏览至“服务设置”  。
1. 在“验证选项”下，选中可供用户使用的方法旁的所有框  。

   ![在多重身份验证服务设置选项卡中配置验证方法](./media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

4. 单击“保存”  。
5. 关闭“服务设置”选项卡  。

### <a name="test-azure-multi-factor-authentication"></a>测试 Azure 多重身份验证

在 InPrivate 或 incognito 模式下打开新的浏览器窗口并浏览到 [https://portal.azure.cn](https://portal.azure.cn)。
   - 使用在本文的先决条件部分创建的测试用户登录，你将发现，现在系统要求你注册并使用 Azure 多重身份验证。
   - 关闭浏览器窗口

## <a name="next-steps"></a>后续步骤

祝贺你，现已在云中设置 Azure 多重身份验证。

若要配置其他设置（例如受信任的 IP、自定义语音消息和欺诈警报），请参阅[配置 Azure 多重身份验证设置](howto-mfa-mfasettings.md)一文。

有关管理 Azure 多重身份验证的用户设置的信息，请参阅[管理云中 Azure 多重身份验证的用户设置](howto-mfa-userdevicesettings.md)一文。

<!-- Update_Description: wording update -->