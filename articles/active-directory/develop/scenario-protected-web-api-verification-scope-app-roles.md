---
title: 使用受保护的 Web API 验证范围和应用角色 | Azure
titleSuffix: Microsoft identity platform
description: 了解如何生成受保护的 Web API 和配置应用程序的代码。
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: df6ebefe3e93e0369ed06d5e2ad50fb0e21515fe
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75776938"
---
# <a name="protected-web-api-verify-scopes-and-app-roles"></a>受保护的 Web API：验证范围和应用角色

本文介绍如何将授权添加到 Web API。 这种保护可确保只有以下对象才能调用 API：

- 代表具有适当范围的用户的应用程序。
- 具有适当应用程序角色的守护程序应用。

> [!NOTE]
> 本文中的代码片段摘自以下可完全正常运行的示例
>
> - GitHub 上的 [ASP.NET Core Web API 增量教程](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/1.%20Desktop%20app%20calls%20Web%20API/TodoListService/Controllers/TodoListController.cs#L37)
> - [ASP.NET Web API 示例](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof/blob/dfd0115533d5a230baff6a3259c76cf117568bd9/TodoListService/Controllers/TodoListController.cs#L48)

若要保护 ASP.NET/ASP.NET Core Web API，需在下列其中一项上添加 `[Authorize]` 属性：

- 控制器本身（若要保护控制器的所有操作）
- API 的单个控制器操作

```csharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

但是，这种保护并不足够。 它只能保证 ASP.NET/ASP.NET Core 验证令牌。 API 需要验证用于调用 Web API 的令牌是否是使用预期的声明请求的，具体而言，这些声明包括：

- 范围（如果代表用户调用 API）。 
- 应用角色（如果可从守护程序应用调用 API）。 

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>验证代表用户调用的 API 中的范围

如果 API 由客户端应用代表用户调用，则该应用需要请求具有该 API 的特定范围的持有者令牌。 （请参阅[代码配置 | 持有者令牌](scenario-protected-web-api-app-configuration.md#bearer-token)。）

```csharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result.
        ...
    }
...
}
```

`VerifyUserHasAnyAcceptedScope` 方法将执行如下所述的操作：

- 验证是否存在名为 `http://schemas.microsoft.com/identity/claims/scope` 或 `scp` 的声明。
- 验证该声明是否具有包含 API 所需范围的值。

```csharp
    /// <summary>
    /// When applied to a <see cref="HttpContext"/>, verifies that the user authenticated in the 
    /// web API has any of the accepted scopes.
    /// If the authenticated user doesn't have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized error with a message noting which scopes are expected in the token.
    /// </summary>
    /// <param name="acceptedScopes">Scopes accepted by this API</param>
    /// <exception cref="HttpRequestException"/> with a <see cref="HttpResponse.StatusCode"/> set to 
    /// <see cref="HttpStatusCode.Unauthorized"/>
    public static void VerifyUserHasAnyAcceptedScope(this HttpContext context,
                                                     params string[] acceptedScopes)
    {
        if (acceptedScopes == null)
        {
            throw new ArgumentNullException(nameof(acceptedScopes));
        }
        Claim scopeClaim = HttpContext?.User
                                      ?.FindFirst("http://schemas.microsoft.com/identity/claims/scope");
        if (scopeClaim == null || !scopeClaim.Value.Split(' ').Intersect(acceptedScopes).Any())
        {
            context.Response.StatusCode = (int)HttpStatusCode.Unauthorized;
            string message = $"The 'scope' claim does not contain scopes '{string.Join(",", acceptedScopes)}' or was not found";
            throw new HttpRequestException(message);
        }
    }
```

此[示例代码](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/Microsoft.Identity.Web/Resource/ScopesRequiredByWebAPIExtension.cs#L47)适用于 ASP.NET Core。 对于 ASP.NET，只需将 `HttpContext.User` 替换为 `ClaimsPrincipal.Current`，并将声明类型 `"http://schemas.microsoft.com/identity/claims/scope"` 替换为 `"scp"`。 （另请参阅本文稍后的代码片段。）

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>验证守护程序应用调用的 API 中的应用角色

如果 Web API 由某个[守护程序应用](scenario-daemon-overview.md)调用，该应用应该对该 Web API 拥有应用程序权限。 我们已在[公开应用程序权限（应用角色）](/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles)中发现，你的 API 会公开此类权限（例如 `access_as_application` 应用角色）。
现在，需要让 API 验证它收到的令牌是否包含 `roles` 声明，以及此声明是否具有所需的值。 执行此验证的代码类似于验证委托权限的代码，不同之处在于，前者不会对 `scopes` 进行测试，而是对 `roles` 测试控制器操作：

```csharp
[Authorize]
public class TodoListController : ApiController
{
    public IEnumerable<TodoItem> Get()
    {
        ValidateAppRole("access_as_application");
        ...
    }
```

`ValidateAppRole()` 方法可能类似于：

```csharp
private void ValidateAppRole(string appRole)
{
    //
    // The `role` claim tells you what permissions the client application has in the service.
    // In this case, we look for a `role` value of `access_as_application`.
    //
    Claim roleClaim = ClaimsPrincipal.Current.FindFirst("roles");
    if (roleClaim == null || !roleClaim.Value.Split(' ').Contains(appRole))
    {
        throw new HttpResponseException(new HttpResponseMessage 
        { StatusCode = HttpStatusCode.Unauthorized, 
            ReasonPhrase = $"The 'roles' claim does not contain '{appRole}' or was not found" 
        });
    }
}
}
```

这一次的代码片段适用于 ASP.NET。 对于 ASP.NET Core，只需将 `ClaimsPrincipal.Current` 替换为 `HttpContext.User`，并将声明名称 `"roles"` 替换为 `"http://schemas.microsoft.com/identity/claims/roles"`。 （另请参阅本文前面的代码片段。）

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>当 Web API 只能由守护程序应用调用时接受仅限应用的令牌

`roles` 声明也用于用户分配模式中的用户。 （请参阅[如何：在应用程序中添加应用角色并在令牌中接收它们](howto-add-app-roles-in-azure-ad-apps.md)。）因此，如果角色可同时分配到用户和应用，只需选中相应的角色就能让应用以用户身份和其他方式登录。 建议为用户和应用声明不同的角色，以避免出现这种混淆。

如果你希望仅允许守护程序应用调用 Web API，请在验证应用角色时添加一个条件，规定该令牌是仅限应用的令牌：

```csharp
string oid = ClaimsPrincipal.Current.FindFirst("oid")?.Value;
string sub = ClaimsPrincipal.Current.FindFirst("sub")?.Value;
bool isAppOnlyToken = oid == sub;
```

选中反向条件将只允许登录用户的应用调用 API。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [转移到生产环境](scenario-protected-web-api-production.md)

<!-- Update_Description: wording update -->