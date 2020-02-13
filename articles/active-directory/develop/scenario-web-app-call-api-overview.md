---
title: 构建调用 Web API 的 Web 应用 - Microsoft 标识平台 | Azure
description: 了解如何构建调用 Web API 的 Web 应用（概述）
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/06/2020
ms.author: v-junlch
ms.custom: aaddev
ms.openlocfilehash: e84cedc9344ac9e869dff908d48e48658e3dbea0
ms.sourcegitcommit: 7c80405a6b48380814b4b414e9f8a5756c007880
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067627"
---
# <a name="scenario-a-web-app-that-calls-web-apis"></a>方案：调用 Web API 的 Web 应用

了解如何构建 Web 应用，使其可将用户在 Microsoft 标识平台上登录，然后代表已登录用户调用 Web API。

## <a name="prerequisites"></a>必备条件

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

此方案假设你已完成以下方案：

> [!div class="nextstepaction"]
> [用于将用户登录的 Web 应用](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>概述

向 Web 应用添加身份验证，以便该应用可以将用户登录并代表已登录用户调用 Web API。

![用于调用 Web API 的 Web 应用](./media/scenario-webapp/web-app.svg)

调用 Web API 的 Web 应用是机密客户端应用程序。
这是它们将机密（应用程序密码或证书）注册到 Azure Active Directory (Azure AD) 的原因。 该机密是在调用 Azure AD 以获取令牌的过程中传入的。

## <a name="specifics"></a>详情

> [!NOTE]
> 向 Web 应用添加登录信息是为了保护 Web 应用本身。 该保护是通过使用“中间件”  库来实现的，而不是使用 Microsoft 身份验证库 (MSAL)。 前面的方案[用于将用户登录的 Web 应用](scenario-web-app-sign-user-overview.md)中涵盖了该主题。
>
> 此方案涵盖了如何从 Web 应用调用 Web API。 你必须获取这些 Web API 的访问令牌。 若要获取那些令牌，请使用 MSAL 库。

此方案的开发涉及以下具体任务：

- 在[应用程序注册](scenario-web-app-call-api-app-registration.md)过程中，必须提供要与 Azure AD 共享的回复 URI、机密或证书。 如果你将应用部署到多个位置，请为每个位置提供此信息。
- [应用程序配置](scenario-web-app-call-api-app-configuration.md)必须提供已在注册应用程序期间与 Azure AD 共享的客户端凭据。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [调用 Web API 的 Web 应用：应用注册](scenario-web-app-call-api-app-registration.md)

<!-- Update_Description: wording update -->