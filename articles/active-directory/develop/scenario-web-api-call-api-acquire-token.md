---
title: 获取调用 Web API 的 Web API 的令牌 | Azure
titleSuffix: Microsoft identity platform
description: 了解如何生成调用 Web API 的 Web API（要求获取应用的令牌）。
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
ms.openlocfilehash: 702e8b60ed23b9dcfa72f481f8ab5d25608f8080
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75776928"
---
# <a name="web-api-that-calls-web-apis---acquire-a-token-for-the-app"></a>调用 Web API 的 Web API - 获取应用的令牌

构建客户端应用程序对象后，使用它来获取可用于调用 Web API 的令牌。

## <a name="code-in-the-controller"></a>控制器中的代码

下面是将在 API 控制器的操作中调用的代码示例，调用下游 API（名为 todolist）。

```csharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...
}
```

`BuildConfidentialClient()` 与你在文章[调用 Web API 的 Web API - 应用配置](scenario-web-api-call-api-app-configuration.md)中看到的类似。 `BuildConfidentialClient()` 使用仅包含一个帐户信息的缓存实例化 `IConfidentialClientApplication`。 该帐户由 `GetAccountIdentifier` 方法提供。

`GetAccountIdentifier` 方法使用与 Web API 收到其 JWT 的用户的标识相关联的声明：

```csharp
public static string GetMsalAccountId(this ClaimsPrincipal claimsPrincipal)
{
 string userObjectId = GetObjectId(claimsPrincipal);
 string tenantId = GetTenantId(claimsPrincipal);

 if (    !string.IsNullOrWhiteSpace(userObjectId)
      && !string.IsNullOrWhiteSpace(tenantId))
 {
  return $"{userObjectId}.{tenantId}";
 }

 return null;
}
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [调用 Web API](scenario-web-api-call-api-call-api.md)

<!-- Update_Description: wording update -->