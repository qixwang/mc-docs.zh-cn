---
title: 生成调用 Web API 的单页应用 - Microsoft 标识平台 | Azure
description: 了解如何生成调用 Web API 的单页应用程序
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/25/2020
ms.author: v-junlch
ms.custom: aaddev
ms.openlocfilehash: 84a68bc6d6c6555abbb13f3a275c9a2fe8640252
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77653153"
---
# <a name="single-page-application-call-a-web-api"></a>单页应用程序：调用 Web API

我们建议你在调用 Web API 之前调用 `acquireTokenSilent` 方法获取或续订访问令牌。 有了令牌后，就可以调用受保护的 Web API 了。

## <a name="call-a-web-api"></a>调用 Web API

# <a name="javascript"></a>[JavaScript](#tab/javascript)

在 HTTP 请求中使用获得的访问令牌作为持有者令牌来调用任何 Web API，例如 Microsoft Graph API。 例如：

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://microsoftgraph.chinacloudapi.cn/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

# <a name="angular"></a>[Angular](#tab/angular)

MSAL Angular 包装器器利用 HTTP 拦截器自动以无提示方式获取访问令牌并将其附加到对 API 的 HTTP 请求中。 有关详细信息，请参阅[获取令牌以调用 API](scenario-spa-acquire-token.md)。

---

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [转移到生产环境](scenario-spa-production.md)

<!-- Update_Description: wording update -->