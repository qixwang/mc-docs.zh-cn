---
title: 将用于登录用户的 Web 应用移到生产环境 - Microsoft 标识平台 | Azure
description: 了解如何构建用于登录用户的 Web 应用（移到生产环境）
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
ms.openlocfilehash: 1a3c3a7a3f9a145d3e06681688c8ab8e74a0a6fa
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77067695"
---
# <a name="web-app-that-signs-in-users-move-to-production"></a>用于登录用户的 Web 应用：移到生产环境

现在你已了解如何获取用于调用 Web API 的令牌，接下来学习如何将其移到生产环境。

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>后续步骤

### <a name="same-site"></a>同一站点

请确保了解 Chrome 浏览器的新版本可能存在的问题

> [!div class="nextstepaction"]
> [如何在 Chrome 浏览器中处理 SameSite Cookie 更改](howto-handle-samesite-cookie-changes-chrome-browser.md)

### <a name="scenario-for-calling-web-apis"></a>用于调用 Web API 的方案

Web 应用登录用户后，它就可以代表已登录用户调用 Web API。 从 Web 应用调用 Web API 是以下方案的目标：

> [!div class="nextstepaction"]
> [用于调用 Web API 的 Web 应用](scenario-web-app-call-api-overview.md)

## <a name="deep-dive-aspnet-core-web-app-tutorial"></a>深入探讨：ASP.NET Core Web 应用教程

了解如何在此 ASP.NET Core 教程中通过其他方法登录用户： 

> [!div class="nextstepaction"]
> [允许 Web 应用通过 Microsoft 面向开发人员的标识平台登录用户和调用 API](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) 

此循序渐进教程提供了 Web 应用的生产就绪代码，包括如何在以下组织中添加使用帐户进行的登录：

- 你的组织
- 多个组织
- 工作或学校帐户
- [Azure AD B2C](/active-directory-b2c/active-directory-b2c-overview)
- 国家云

## <a name="sample-code-java-web-app"></a>示例代码：Java Web 应用

通过 GitHub 上的此示例详细了解 Java Web 应用： 

> [!div class="nextstepaction"]
> [一个 Java Web 应用程序，该应用程序使用 Microsoft 标识平台登录用户并调用 Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp)

<!-- Update_Description: wording update -->