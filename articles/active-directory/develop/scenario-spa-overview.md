---
title: JavaScript 单页应用方案 - Microsoft 标识平台 | Azure
description: 了解如何使用 Microsoft 标识平台构建单页应用程序（方案概述）。
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/06/2020
ms.author: v-junlch
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: b969136010928c0c9f0071fcabe0d93491a96187
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75777041"
---
# <a name="scenario-single-page-application"></a>方案：单页应用程序

了解生成单页应用程序 (SPA) 所需的一切。

## <a name="prerequisites"></a>必备条件

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>入门

可以按 JavaScript SPA 快速入门创建第一个应用程序：

> [!div class="nextstepaction"]
> [快速入门：单页应用程序](./quickstart-v2-javascript.md)

## <a name="overview"></a>概述

许多新式 Web 应用程序都是作为客户端单页应用程序构建的。 开发人员使用 JavaScript 或 SPA 框架（如 Angular、Vue.js 和 React.js）进行编写。 这些应用程序在 Web 浏览器上运行，与传统的服务器端 Web 应用程序相比，它们具有不同的身份验证特征。 

Microsoft 标识平台可让单页应用程序使用 [OAuth 2.0 隐式流](./v2-oauth2-implicit-grant-flow.md)将用户登录，并获取用于访问后端服务或 Web API 的令牌。 隐式流允许应用程序获取 ID 令牌来表示已经过身份验证的用户以及调用受保护 API 所需的访问令牌。

![单页应用程序](./media/scenarios/spa-app.svg)

此身份验证流不包括使用 Electron 和 React-Native 之类的跨平台 JavaScript 框架的应用程序方案。 它们需要使用其他功能才能与本机平台交互。

## <a name="specifics"></a>详情

若要为应用程序启用此方案，需要：

* 通过 Azure Active Directory (Azure AD) 进行应用程序注册。 此注册需要启用隐式流并设置重定向 URI，方便令牌返回。
* 带有注册的应用程序属性（例如应用程序 ID）的应用程序配置。
* 使用 Microsoft 身份验证库 (MSAL) 执行身份验证流以登录并获取令牌。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用注册](scenario-spa-app-registration.md)

<!-- Update_Description: wording update -->