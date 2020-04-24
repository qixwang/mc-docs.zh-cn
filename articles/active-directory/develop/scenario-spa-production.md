---
title: 将单页应用移到生产环境 - Microsoft 标识平台 | Azure
description: 了解如何生成单页应用程序（移到生产环境）
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
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0865a3780072afed33808f708f1449cffe33ea9c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75776931"
---
# <a name="single-page-application-move-to-production"></a>单页应用程序：移到生产环境

现在你已了解如何获取用于调用 Web API 的令牌，接下来了解如何移到生产环境。

## <a name="improve-your-app"></a>改进应用

[启用日志记录](msal-logging.md)使应用生产环境准备就绪。

## <a name="test-your-integration"></a>测试集成

按照 [Microsoft 标识平台集成清单](identity-platform-integration-checklist.md)测试你的集成。

## <a name="next-steps"></a>后续步骤

深入了解快速入门示例，该示例说明了如何使用 MSAL.js 登录用户并获取访问令牌以调用 Microsoft Graph API 的代码：

> [!div class="nextstepaction"]
> [JavaScript SPA 教程](./tutorial-v2-javascript-spa.md)

演示如何使用 MSAL.js 为自己的后端 Web API 获取令牌的示例：

> [!div class="nextstepaction"]
> [使用 ASP.NET 后端的 SPA](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2)

演示如何使用 MSAL.js 在向 Azure Active Directory B2C (Azure AD B2C) 注册的应用中登录用户的示例：

> [!div class="nextstepaction"]
> [使用 Azure AD B2C 的 SPA](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)

<!-- Update_Description: wording update -->