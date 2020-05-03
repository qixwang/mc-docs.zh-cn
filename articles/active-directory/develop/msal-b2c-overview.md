---
title: 将 MSAL 与 Azure Active Directory B2CLearn 配合使用 | Azure
titleSuffix: Microsoft identity platform
description: 借助适用于 JavaScript 的 Microsoft 身份验证库 (MSAL.js)，应用程序可以使用 Azure AD B2C 并获取用于调用受保护 Web API 的令牌。 这些 Web API 可以是 Microsoft Graph、其他 Microsoft API、其他来源的 Web API 或你自己的 Web API。
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/22/2020
ms.author: v-junlch
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: a80e801b252b16f32b6db2fd63c241300833e6f7
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126513"
---
# <a name="use-microsoft-authentication-library-for-javascript-to-work-with-azure-active-directory-b2c"></a>通过适用于 JavaScript 的 Microsoft 身份验证库来使用 Azure Active Directory B2C

借助[适用于 JavaScript 的 Microsoft 身份验证库 (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js)，JavaScript 应用程序开发人员可以使用 [Azure Active Directory B2C (Azure AD B2C)](/active-directory-b2c/) 通过社交和本地标识对用户进行身份验证。 使用作为标识管理服务的 Azure AD B2C，可以自定义和控制客户的注册和登录方式，并在他们使用你的应用程序时管理其配置文件。

使用 Azure AD B2C，还可以设置应用程序在身份验证过程中显示的品牌并自定义其 UI，以便为客户提供无缝体验。

本文演示如何通过 MSAL.js 来使用 Azure AD B2C，并汇总了应该注意的要点。 有关完整的讨论和教程，请查阅 [Azure AD B2C 文档](/active-directory-b2c/overview)。

## <a name="prerequisites"></a>先决条件

如果你尚未创建自己的 [Azure AD B2C 租户](/active-directory-b2c/tutorial-create-tenant)，现在请开始创建一个（也可以使用现有的 Azure AD B2C 租户，如果有的话）。

本演示文章包含两个部分：

- 如何保护 Web API。
- 如何注册一个单页应用程序以验证并调用该 Web API  。

## <a name="nodejs-web-api"></a>Node.js Web API

> [!NOTE]
> 适用于 Node 的 MSAL.js 目前仍在开发中（请参阅[路线图](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki#roadmap)）。 在此期间，我们建议使用 [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad)，它是由 Microsoft 开发并提供支持的适用于 Node.js 的身份验证库。

以下步骤演示 Web API 如何使用 Azure AD B2C 来保护自身，并将所选范围公开给客户端应用程序  。

### <a name="step-1-register-your-application"></a>步骤 1：注册应用程序

若要使用 Azure AD B2C 保护 Web API，首先需要注册该 Web API。 请遵循[注册应用程序](/active-directory-b2c/add-web-application?tabs=applications)来执行详细步骤。

### <a name="step-2-download-the-sample-application"></a>步骤 2：下载示例应用程序

下载示例作为 zip 文件，或从 GitHub 克隆：

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

### <a name="step-3-configure-authentication"></a>步骤 3：配置身份验证

1. 在示例中打开 `config.js` 文件。

2. 注册应用程序时，请使用前面获取的应用程序凭据配置示例。 更改以下代码行：将值替换为你的 clientID、主机、tenantId 和策略的名称。

```JavaScript
const clientID = "<Application ID for your Node.js web API - found on Properties page in Azure portal e.g. 93733604-cc77-4a3c-a604-87084dd55348>";
const b2cDomainHost = "<Domain of your B2C host eg. fabrikamb2c.b2clogin.com>";
const tenantId = "<your-tenant-ID>.partner.onmschina.cn"; // Alternatively, you can use your Directory (tenant) ID (GUID)
const policyName = "<Name of your sign in / sign up policy, e.g. B2C_1_signupsignin1>";
```

有关详细信息，请查看此 [Node.js B2C Web API 示例](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)。

---

## <a name="javascript-spa"></a>JavaScript SPA

以下步骤演示单页应用程序如何使用 Azure AD B2C 进行注册和登录，并调用受保护的 Web API  。

### <a name="step-1-register-your-application"></a>步骤 1：注册应用程序

若要实现身份验证，首先需要注册你的应用程序。 请遵循[注册应用程序](/active-directory-b2c/tutorial-register-applications)来执行详细步骤。

### <a name="step-2-download-the-sample-application"></a>步骤 2：下载示例应用程序

下载示例作为 zip 文件，或从 GitHub 克隆：

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>步骤 3：配置身份验证

配置应用程序时有两个关注点：

- 配置 API 终结点和公开的范围
- 配置身份验证参数和令牌范围

1. 在示例中打开 `apiConfig.js` 文件。

2. 注册 Web API 时，请使用前面获取的参数配置示例。 更改以下代码行：将值替换为你的 Web API 的地址和公开的范围。

   ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    const apiConfig = {
        b2cScopes: ["https://fabrikamb2c.partner.onmschina.cn/helloapi/demo.read"], //API scopes you exposed during api registration
        webApi: "https://fabrikamb2chello.chinacloudsites.cn/hello"
    };
   ```

3. 在示例中打开 `authConfig.js` 文件。

4. 注册单页应用程序时，请使用前面获取的参数配置示例。 更改以下代码行：将值替换为你的 ClientId、颁发机构元数据和令牌请求范围。

   ```javascript
    // Config object to be passed to Msal on creation.
    const msalConfig = {
        auth: {
            clientId: "e760cab2-b9a1-4c0d-86fb-ff7084abd902",
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.partner.onmschina.cn/B2C_1_signupsignin1",
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage", // This configures where your cache will be stored
            storeAuthStateInCookie: false // Set this to "true" to save cache in cookies
        }
    };

    // Add here scopes for id token to be used at the MS Identity Platform endpoint
    const loginRequest = {
        scopes: ["openid", "profile"],
    };
   ```

有关详细信息，请查看此 [JavaScript B2C 单页应用程序示例](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)。

---

## <a name="next-steps"></a>后续步骤

了解有关以下方面的详细信息：
- [用户流](/active-directory-b2c/tutorial-create-user-flows)
- [自定义策略](/active-directory-b2c/custom-policy-get-started)
- [UX 自定义](/active-directory-b2c/custom-policy-configure-user-input)

