---
title: Azure 多重身份验证 - 工作原理
description: Azure 多重身份验证可帮助保护对数据和应用程序的访问，同时可以满足用户对简单登录过程的需求。
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
origin.date: 07/11/2018
ms.date: 09/04/2018
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 8a778b54c30a81bc8daf7e1dee7e9718aa4aeb70
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63822638"
---
# <a name="how-azure-multi-factor-authentication-works"></a>Azure 多重身份验证的工作原理
双重验证的安全性在于它的分层方法。 破坏多因素身份验证系统对于攻击者来说是巨大的挑战。 即使攻击者设法得到用户的密码，如果没有同时占有可信设备也没有用处。 

![验证](./media/concept-mfa-howitworks/howitworks.png)

Azure 多重身份验证可帮助保护对数据和应用程序的访问，同时可以满足用户对简单登录过程的需求。  它通过要求第二种形式的身份验证提供额外的安全性，并通过一系列简单的身份验证选项提供增强式身份验证。


## <a name="methods-available-for-two-step-verification"></a>可用于双重验证的方法
当用户登录时，系统会将额外的身份验证发送给该用户。  以下是可用于这种二次身份验证的方法列表。

| 验证方法 | 说明 |
| --- | --- |
| 电话呼叫 |致电用户已注册的电话号码。 用户在必要时输入 PIN，再按 # 键。 |
| 短信 |向用户的移动电话发送包含 6 位数验证码的短信。 用户在登录页上输入此验证码。 |
| 移动应用通知 |向用户的智能手机发送验证请求。 用户在必要时输入 PIN，再在移动应用上选择“验证”  。 |
| 移动应用验证码 |在用户智能手机上运行的移动应用将显示验证码，每 30 秒更改一次。 找到最新验证码后，用户在登录页上输入验证码。 |

Azure 多重身份验证为云和服务器提供了可选择的验证方法。 可以选择对用户使用哪些方法：电话呼叫、短信、应用通知还是应用验证码。 有关更多信息，请参阅[可选择的验证方法](howto-mfa-mfasettings.md#selectable-verification-methods)。

## <a name="next-steps"></a>后续步骤

- 阅读有关 [Azure 多重身份验证的不同版本和使用方法](concept-mfa-licensing.md)的信息

- 选择是否将 Azure MFA 部署[在云中](howto-mfa-getstarted.md)

- 阅读[常见问题解答](multi-factor-authentication-faq.md)

<!-- Update_Description: update metedata properties -->

