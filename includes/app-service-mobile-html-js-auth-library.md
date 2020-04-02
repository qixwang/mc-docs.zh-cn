---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
origin.date: 08/23/2018
ms.date: 09/11/2019
ms.author: v-tawe
ms.openlocfilehash: bc2c1a28c4827c1d01e611458ab50a4faadbd9e5
ms.sourcegitcommit: b2f2bb08ab1b5ccb3c596d84b3b6ddca5bba3903
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80151743"
---
### <a name="how-to-authenticate-with-a-provider-server-flow"></a><a name="server-auth"></a>如何：使用提供程序（服务器流）进行身份验证
若要让移动应用管理应用中的身份验证过程，必须向标识提供者注册应用。 然后，需要在 Azure 应用服务中配置提供者提供的应用程序 ID 和机密。
有关详细信息，请参阅 [向应用程序添加身份验证](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md)教程。

注册标识提供者后，使用提供者的名称调用 `.login()` 方法。 例如，若要使用 MicrosoftAccount 登录，请使用以下代码：

```javascript
client.login("microsoftaccount").done(function (results) {
     alert("You are now signed in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

提供者的有效值为“aad”和“microsoftaccount”。

在这种情况下，由 Azure 应用服务管理 OAuth 2.0 身份验证流程。  它显示所选提供者的登录页，并在使用标识提供者成功登录后生成应用服务身份验证令牌。 login 函数在完成时返回一个 JSON 对象，该对象分别在 userId 和 authenticationToken 字段中公开用户 ID 和应用服务身份验证令牌。 可以缓存此令牌，并在它过期之前重复使用。

### <a name="how-to-authenticate-with-a-provider-client-flow"></a><a name="client-auth"></a>如何：使用提供程序（客户端流）进行身份验证

用户的应用还能够独立联系标识提供者，并将返回的令牌提供给应用服务以进行身份验证。 使用此客户端流可为用户提供单一登录体验，或者从标识提供者中检索其他用户数据。

#### <a name="social-authentication-basic-example"></a>社交身份验证基本示例

此示例使用 MicrosoftAccount 客户端 SDK 进行身份验证：

```javascript
client.login(
     "microsoftaccount",
     {"access_token": token})
.done(function (results) {
     alert("You are now signed in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});

```
此示例假定由相应的提供程序 SDK 提供的令牌存储在令牌变量中。

### <a name="how-to-obtain-information-about-the-authenticated-user"></a><a name="auth-getinfo"></a>如何：获取已经过身份验证的用户的相关信息

可以结合任何 AJAX 库使用 HTTP 调用，从 `/.auth/me` 终结点检索身份验证信息。  确保将 `X-ZUMO-AUTH` 标头设置为身份验证令牌。  身份验证令牌存储在 `client.currentUser.mobileServiceAuthenticationToken`中。  例如，若要使用提取 API：

```javascript
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

提取内容以 [npm 包](https://www.npmjs.com/package/whatwg-fetch)的形式提供，或者可以通过浏览器从 [CDNJS](https://cdnjs.com/libraries/fetch) 下载。 也可以使用 jQuery 或其他 AJAX API 提取信息。  数据作为 JSON 对象接收。