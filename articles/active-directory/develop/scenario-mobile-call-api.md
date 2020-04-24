---
title: 从移动应用调用 Web API | Azure
titleSuffix: Microsoft identity platform
description: 了解如何构建用于调用 Web API 的移动应用。 （调用 Web API。）
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
ms.date: 02/25/2020
ms.author: v-junlch
ms.reviwer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 989c6a1b73e35322f6bd690a41e5b478af09668f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77653163"
---
# <a name="call-a-web-api-from-a-mobile-app"></a>从移动应用调用 Web API

在你的应用将用户登录并收到令牌以后，Microsoft 身份验证库 (MSAL) 会公开有关用户、用户的环境以及所颁发令牌的信息。 应用可以使用这些值来调用 Web API 或向用户显示欢迎消息。

在本文中，我们首先看一下 MSAL 结果。 接下来，我们看看如何使用 `AuthenticationResult` 或 `result` 中的访问令牌来调用受保护的 Web API。

## <a name="msal-result"></a>MSAL 结果
MSAL 提供以下值： 

- `AccessToken` 在 HTTP 持有者请求中调用受保护的 Web API。
- `IdToken` 包含有关已登录用户的有用信息。 此信息包括用户的名称、主租户，以及存储的唯一标识符。
- `ExpiresOn` 是令牌的过期时间。 MSAL 处理应用的自动刷新。
- `TenantId` 是用户登录到的租户的标识符。 该值不标识用户的主租户。  
- `Scopes` 指示通过你的令牌授予的作用域。 授予的作用域可能是请求的作用域的子集。

MSAL 还提供了 `Account` 值的抽象。 `Account` 值表示当前用户的已登录帐户：

- `HomeAccountIdentifier` 标识用户的主租户。
- `UserName` 是用户的首选用户名。 对于 Azure AD B2C 用户，此值可能为空。
- `AccountIdentifier` 标识已登录用户。 大多数情况下，此值与 `HomeAccountIdentifier` 值相同，除非用户是另一租户中的来宾。

## <a name="call-an-api"></a>调用 API

有了访问令牌以后，可以调用 Web API。 应用将使用此令牌生成一个 HTTP 请求，然后运行该请求。

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing.
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called Graph. Process data and send to UI.
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error.
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put access token in HTTP request.
                headers.put("Authorization", "Bearer " + authResult.getAccessToken());
                return headers;
            }
        };

        request.setRetryPolicy(new DefaultRetryPolicy(
                3000,
                DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        queue.add(request);
```

### <a name="msal-for-ios-and-macos"></a>适用于 iOS 和 MacOS 的 MSAL

用于获取令牌的方法返回一个 `MSALResult` 对象。 `MSALResult` 公开 `accessToken` 属性。 可以使用 `accessToken` 来调用 Web API。 将此属性添加到 HTTP 授权标头，然后再进行调用来访问受保护的 Web API。

```objc
NSMutableURLRequest *urlRequest = [NSMutableURLRequest new];
urlRequest.URL = [NSURL URLWithString:"https://contoso.api.com"];
urlRequest.HTTPMethod = @"GET";
urlRequest.allHTTPHeaderFields = @{ @"Authorization" : [NSString stringWithFormat:@"Bearer %@", accessToken] };
        
NSURLSessionDataTask *task =
[[NSURLSession sharedSession] dataTaskWithRequest:urlRequest
     completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {}];
[task resume];
```

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]
     
let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

### <a name="xamarin"></a>Xamarin

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

## <a name="make-several-api-requests"></a>发出多个 API 请求

如需多次调用同一 API，或者需要调用多个 API，则在构建应用时请考虑以下主题：

- **增量许可**：Microsoft 标识平台允许应用在需要权限的时候获取用户许可，而不是在开始时统一这样做。 应用在每次准备调用 API 时，应该只请求它需要的作用域。

## <a name="call-several-apis-by-using-incremental-consent"></a>使用增量许可调用若干 API

如果需要为同一用户调用多个 API，则在为用户获取令牌后，可以通过随后调用 `AcquireTokenSilent` 获取令牌来避免重复要求用户提供凭据：

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

以下情况需要交互：

- 用户已许可第一个 API，但现在需要许可更多范围。 在这种情况下，你将使用增量许可。
- 第一个 API 不需要多重身份验证，但下一个 API 需要。

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [转移到生产环境](scenario-mobile-production.md)

<!-- Update_Description: wording update -->