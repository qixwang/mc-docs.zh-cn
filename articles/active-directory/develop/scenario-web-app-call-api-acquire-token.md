---
title: 在调用 Web API 的 Web 应用中获取令牌 - Microsoft 标识平台 | Azure
description: 了解如何生成调用 Web API 的 Web 应用（获取应用的令牌）
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
ms.openlocfilehash: ecc4287ef0d54fd2405782f86cb29812a0013e98
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75776926"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>调用 Web API 的 Web 应用 - 获取应用的令牌

生成客户端应用程序对象以后，你将使用它来获取令牌以调用 Web API。 然后，在 ASP.NET 或 ASP.NET Core 的控制器中调用 Web API。 具体操作是：

- 使用令牌缓存获取 Web API 的令牌。 若要获取此令牌，请调用 `AcquireTokenSilent`。
- 使用访问令牌调用受保护的 API。

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

控制器方法受 `[Authorize]` 属性的保护，该属性会强制经身份验证的用户使用 Web 应用。 下面是用于调用 Microsoft Graph 的代码。

```csharp
[Authorize]
public class HomeController : Controller
{
 readonly ITokenAcquisition tokenAcquisition;

 public HomeController(ITokenAcquisition tokenAcquisition)
 {
  this.tokenAcquisition = tokenAcquisition;
 }

 // Code for the controller actions(see code below)

}
```

`ITokenAcquisition` 服务是由 ASP.NET 通过依赖项注入来注入的。


下面是 HomeController 操作的简化代码，该操作获取调用 Microsoft Graph 所需的令牌。

```csharp
public async Task<IActionResult> Profile()
{
 // Acquire the access token
 string[] scopes = new string[]{"https://microsoftgraph.chinacloudapi.cn/user.read"};
 string accessToken = await tokenAcquisition.GetAccessTokenOnBehalfOfUserAsync(scopes);

 // use the access token to call a protected web API
 HttpClient client = new HttpClient();
 client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
 string json = await client.GetStringAsync(url);
}
```

若要更全面地了解此方案所需的代码，请参阅 [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) 教程的阶段 2 ([2-1-Web App Calls Microsoft Graph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)) 步骤。

有许多其他的复杂情况，例如：

- 调用多个 API，
- 处理增量许可。

这些高级步骤在教程 [3-WebApp-multi-APIs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/3-WebApp-multi-APIs) 的第 3 章中进行处理

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET 中的情况类似：

- 受 [Authorize] 属性保护的控制器操作会提取控制器的 `ClaimsPrincipal` 成员的租户 ID 和用户 ID。 （ASP.NET 使用 `HttpContext.User`。）
- 然后，它会构建一个 MSAL.NET `IConfidentialClientApplication`。
- 最后，它调用机密客户端应用程序的 `AcquireTokenSilent` 方法。

此代码类似于为 ASP.NET Core 显示的代码。

# <a name="javatabjava"></a>[Java](#tab/java)

在 Java 示例中，调用 API 的代码位于 getUsersFromGraph 方法 [AuthPageController.java#L62](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L62) 中。

它尝试调用 `getAuthResultBySilentFlow`。 如果用户需要许可更多作用域，则该代码会处理 `MsalInteractionRequiredException` 以质询用户。

```java
@RequestMapping("/msal4jsample/graph/me")
public ModelAndView getUserFromGraph(HttpServletRequest httpRequest, HttpServletResponse response)
        throws Throwable {

    IAuthenticationResult result;
    ModelAndView mav;
    try {
        result = authHelper.getAuthResultBySilentFlow(httpRequest, response);
    } catch (ExecutionException e) {
        if (e.getCause() instanceof MsalInteractionRequiredException) {

            // If silent call returns MsalInteractionRequired, then redirect to Authorization endpoint
            // so user can consent to new scopes
            String state = UUID.randomUUID().toString();
            String nonce = UUID.randomUUID().toString();

            SessionManagementHelper.storeStateAndNonceInSession(httpRequest.getSession(), state, nonce);

            String authorizationCodeUrl = authHelper.getAuthorizationCodeUrl(
                    httpRequest.getParameter("claims"),
                    "https://microsoftgraph.chinacloudapi.cn/user.read",
                    authHelper.getRedirectUriGraph(),
                    state,
                    nonce);

            return new ModelAndView("redirect:" + authorizationCodeUrl);
        } else {

            mav = new ModelAndView("error");
            mav.addObject("error", e);
            return mav;
        }
    }

    if (result == null) {
        mav = new ModelAndView("error");
        mav.addObject("error", new Exception("AuthenticationResult not found in session."));
    } else {
        mav = new ModelAndView("auth_page");
        setAccountInfo(mav, httpRequest);

        try {
            mav.addObject("userInfo", getUserInfoFromGraph(result.accessToken()));

            return mav;
        } catch (Exception e) {
            mav = new ModelAndView("error");
            mav.addObject("error", e);
        }
    }
    return mav;
}
// Code omitted here.
```

# <a name="pythontabpython"></a>[Python](#tab/python)

在 python 示例中，调用 Microsoft graph 的代码位于 [app.py#L53-L62](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/48637475ed7d7733795ebeac55c5d58663714c60/app.py#L53-L62) 中。

它尝试从令牌缓存获取令牌，然后在设置授权标头后调用 Web API。 如果它无法这样做，它将重新登录用户。

```python
@app.route("/graphcall")
def graphcall():
    token = _get_token_from_cache(app_config.SCOPE)
    if not token:
        return redirect(url_for("login"))
    graph_data = requests.get(  # Use token to call downstream service
        app_config.ENDPOINT,
        headers={'Authorization': 'Bearer ' + token['access_token']},
        ).json()
    return render_template('display.html', result=graph_data)
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [调用 Web API](scenario-web-app-call-api-call-api.md)

<!-- Update_Description: wording update -->