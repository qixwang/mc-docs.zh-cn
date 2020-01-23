---
title: 注册用于调用 Web API 的桌面应用 - Microsoft 标识平台 | Azure
description: 了解如何构建调用 Web API 的桌面应用（应用注册）
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
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
ms.openlocfilehash: 76ca9dff3fe88f0f9db3496ade718182d59e1664
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75776954"
---
# <a name="desktop-app-that-calls-web-apis---app-registration"></a>调用 Web API 的桌面应用 - 应用注册

本文涵盖了桌面应用程序的应用注册详细信息。

## <a name="supported-accounts-types"></a>支持的帐户类型

在桌面应用程序中支持的帐户类型取决于你想要启用的体验。 由于此关系，支持的帐户类型取决于要使用的流。

### <a name="audience-for-interactive-token-acquisition"></a>交互式令牌获取的受众

如果桌面应用程序使用交互式身份验证，则可通过任何[帐户类型](quickstart-register-app.md#register-a-new-application-using-the-azure-portal)登录用户。

### <a name="audience-for-desktop-app-silent-flows"></a>桌面应用无提示流的受众

- 若要使用集成 Windows 身份验证或用户名/密码，应用程序需在你自己的租户（LOB 开发人员）或 Azure Active Directory 组织（ISV 方案）中登录用户。
- 如果使用传入 B2C 颁发机构和策略的社交标识来登录用户，则只能使用交互式和用户-密码身份验证。

## <a name="redirect-uris"></a>重定向 URI

可以在桌面应用程序中使用的重定向 URI 将取决于要使用的流。

- 如果使用**交互式身份验证**，则需使用 `https://login.partner.microsoftonline.cn/common/oauth2/nativeclient`。 单击应用程序的“身份验证”部分中的相应 URL 即可实现此配置  。
  
  > [!IMPORTANT]
  > 目前，在默认情况下，MSAL.NET 会在 Windows 上运行的桌面应用程序中使用另一重定向 URI (`urn:ietf:wg:oauth:2.0:oob`)。 我们在将来需要更改此默认设置，因此建议你使用 `https://login.partner.microsoftonline.cn/common/oauth2/nativeclient`

- 如果要生成用于 macOS 的本机 Objective-C 或 Swift 应用，则需要基于应用程序的捆绑包标识符以以下格式注册 redirectUri：**msauth.<your.app.bundle.id>://auth**（将 <your.app.bundle.id> 替换为应用程序的捆绑包标识符）
- 如果应用仅使用集成 Windows 身份验证或用户名/密码，则不需为应用程序注册重定向 URI。 这些流会往返 Microsoft 标识平台 v2.0 终结点，因此不会在任何特定 URI 上回调你的应用程序。
- 为了将设备代码流、集成 Windows 身份验证和用户名/密码与也没有重定向 URI 的机密客户端应用程序流（在守护程序应用程序中使用的客户端凭据流）区分开来，你需要表示你的应用程序是公共客户端应用程序。 若要实现此配置，请转到应用程序的“身份验证”部分。  然后，在“默认客户端类型”段落的“高级设置”子部分中，针对“将应用程序视为公共客户端”问题选择“是”。    

  ![允许公共客户端](./media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>API 权限

桌面应用程序为已登录用户调用 API。 它们需要请求委托的权限。 但是，它们不能请求应用程序权限，这些只能在[守护程序应用程序](scenario-daemon-overview.md)中处理。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [桌面应用 - 应用配置](scenario-desktop-app-configuration.md)

<!-- Update_Description: wording update -->