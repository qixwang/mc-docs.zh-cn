---
title: 身份验证和授权 | Azure
titleSuffix: Microsoft identity platform
description: 了解 Microsoft 标识平台 (v2.0) 中的身份验证和授权的基础知识。
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/25/2020
ms.author: v-junlch
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: e2a67b61edb3bda74c0da5d924fe882b0b0b8df1
ms.sourcegitcommit: 0130a709d934d89db5cccb3b4997b9237b357803
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84186980"
---
# <a name="authentication-vs-authorization"></a>身份验证和授权

本文定义了身份验证和授权，并简要介绍了如何使用 Microsoft 标识平台对 Web 应用、Web API 或调用受保护的 Web API 的应用中的用户进行身份验证和授权。 如果你看到不熟悉的术语，请尝试查看我们的[术语表](developer-glossary.md)。

## <a name="authentication"></a>身份验证

**身份验证**就是证明自己的身份的过程。 身份验证有时缩写为 AuthN。 Microsoft 标识平台实现用于处理身份验证的 [OpenID Connect](https://openid.net/connect/) 协议。

## <a name="authorization"></a>授权

**授权**是指授予经过身份验证的一方执行某项操作的权限的措施。 它指定了你可以访问的数据，以及可对该数据执行的操作。 授权有时缩写为 AuthZ。 Microsoft 标识平台实现用于处理授权的 [OAuth 2.0](https://oauth.net/2/) 协议。

## <a name="authentication-and-authorization-using-microsoft-identity-platform"></a>使用 Microsoft 标识平台进行身份验证和授权

无需创建每个需要维护自身用户名和密码信息的应用（需要在多个应用中添加或删除用户时，这会产生较高的管理负担），而可以让应用将这种责任委托给一个集中式标识提供者。

Azure Active Directory (Azure AD) 是云中的集中式标识提供者。 向其委托身份验证和授权可以实现条件访问策略等方案以要求用户位于特定位置、使用多重身份验证，以及允许用户登录一次后自动登录到共享同一集中式目录的所有 Web 应用。 此功能称为单一登录 (SSO)。

Microsoft 标识平台通过以下方式简化了对应用程序开发人员的身份验证和授权：将标识提供为一项服务、支持行业标准协议（例如 OAuth 2.0 和 OpenID Connect），并提供用于不同平台的开放源代码库来帮助你快速开始编写代码。 开发人员可以通过它来生成应用程序，以便进行所有 Microsoft 标识的登录，以及获取令牌来调用 [Microsoft Graph](https://developer.microsoft.com/graph/)、其他 Microsoft API 或者开发人员生成的 API。 有关详细信息，请参阅 [Microsoft 标识平台的发展](about-microsoft-identity-platform.md)。

下面简单比较了一下 Microsoft 标识平台使用的各种协议：

* **OAuth 与OpenID Connect**：OAuth 用于授权，而 OpenID Connect (OIDC) 用于验证。 OpenID Connect 的构建基于 OAuth 2.0，因此这两者的术语和流是相近的。 甚至可以在一个请求中（使用 OpenID Connect）对用户进行身份验证和（使用 OAuth 2.0）获得访问该用户拥有的受保护资源的权限。 有关详细信息，请参阅 [OAuth 2.0 和 OpenID Connect 协议](active-directory-v2-protocols.md)和 [OpenID Connect 协议](v2-protocols-oidc.md)。
* **OAuth 与SAML**：OAuth 用于授权，而 SAML 用于身份验证。 要获取有关如何将两种协议一起用于（使用 SAML），从而同时对用户进行身份验证和（使用 OAuth 2.0）获得访问受保护资源的权限的详细信息，请参阅 [Microsoft 标识平台和 OAuth 2.0 SAML 持有者断言流](v2-saml-bearer-assertion.md)。
* **OpenID Connect 与SAML**：OpenID Connect 和 SAML 均用于对用户进行身份验证，并用于启用单一登录。 SAML 身份验证通常与标识提供者（例如，联合到 Azure AD 的 Active Directory 联合身份验证服务 (ADFS)）一起使用，因此经常用于企业应用程序。 OpenID Connect 通常用于仅位于云中的应用，例如移动应用、网站和 Web API。

## <a name="next-steps"></a>后续步骤

有关涉及身份验证和授权基础知识的其他主题，请参阅以下资源：

* 参阅[安全令牌](security-tokens.md)，了解如何在身份验证和授权中使用访问令牌、刷新令牌和 ID 令牌。
* 参阅[应用程序模型](application-model.md)，了解注册应用程序以便它可以与 Microsoft 标识平台集成的过程。
* 参阅[应用登录流](app-sign-in-flow.md)，了解 Microsoft 标识平台中 Web、桌面和移动应用的登录流。

要了解有关 Microsoft 标识平台实现的协议的详细信息：

* 有关 OpenID Connect 和 OAuth 2.0 标准的详细信息，请参阅 [Microsoft 标识平台上的 OAuth 2.0 和 OpenID Connect 协议](active-directory-v2-protocols.md)。


