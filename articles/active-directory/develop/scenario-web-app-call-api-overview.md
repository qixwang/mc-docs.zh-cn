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
ms.date: 01/06/2020
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8b8bfb8295d82991962755cf915272262085d7b
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75776925"
---
# <a name="scenario-web-app-that-calls-web-apis"></a>方案：用于调用 Web API 的 Web 应用

了解如何构建 Web 应用，使其可让用户在 Microsoft 标识平台上登录，然后代表已登录用户调用 Web API。

## <a name="prerequisites"></a>必备条件

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

此方案假设你已完成以下方案：

> [!div class="nextstepaction"]
> [用于登录用户的 Web 应用](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>概述

向 Web 应用添加身份验证，这样该应用就可以登录用户并代表已登录用户调用 Web API。

![用于调用 Web API 的 Web 应用](./media/scenario-webapp/web-app.svg)

用于调用 Web API 的 Web 应用：

- 是机密客户端应用程序。
- 这是它们将机密（应用程序密码或证书）注册到 Azure AD 的原因。 该机密是在调用 Azure AD 以获取令牌的过程中传入的

## <a name="specifics"></a>详情

> [!NOTE]
> 向 Web 应用添加登录并不使用 MSAL 库，因为这是为了保护 Web 应用。 保护库由名为中间件的库来实现。 这是上一方案（[将用户登录到 Web 应用](scenario-web-app-sign-user-overview.md)）的目标
>
> 从 Web 应用调用 Web API 时，需获取这些 Web API 的访问令牌。 可以使用 MSAL 库来获取这些令牌。

因此，开发人员对此方案的端到端体验具有以下特点：

- 在[应用程序注册期间](scenario-web-app-call-api-app-registration.md)，需提供一个或多个（如果将应用部署到多个位置）需与 Azure AD 共享的回复 URI、机密或证书。
- [应用程序配置](scenario-web-app-call-api-app-configuration.md)需提供客户端凭据，这些凭据是在应用程序注册期间与 Azure AD 共享的

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用注册](scenario-web-app-call-api-app-registration.md)

<!-- Update_Description: wording update -->