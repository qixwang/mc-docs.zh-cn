---
title: 调用 Web API 的 Web API - Microsoft 标识平台 | Azure
description: 了解如何构建调用 Web API 的 Web API。
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
ms.openlocfilehash: 5f258414cfb0afb2ad2aa150d5ccdc92adb01806
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75777035"
---
# <a name="web-api-that-calls-web-apis---call-an-api"></a>调用 Web API 的 Web API - 调用 API

有了令牌后，就可以调用受保护的 Web API 了。 这是通过 ASP.NET/ASP.NET Core Web API 的控制器完成的。

## <a name="controller-code"></a>控制器代码

下面是[受保护的 Web API 调用 Web API - 获取令牌](scenario-web-api-call-api-acquire-token.md)中演示的示例代码的延续，在 API 控制器的操作中调用，调用下游 API（名为 todolist）。

获取令牌后，将其用作持有者令牌以调用下游 API。

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

// Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [转移到生产环境](scenario-web-api-call-api-production.md)

<!-- Update_Description: wording update -->