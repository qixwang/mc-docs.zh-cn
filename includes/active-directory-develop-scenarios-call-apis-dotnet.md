---
title: include 文件
description: include 文件
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/08/2020
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 16741dae9a0bacafb6e2e1971dd8f79d770e2701
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75777044"
---
### <a name="authenticationresult-properties-in-msalnet"></a>MSAL.NET 中的 AuthenticationResult 属性

用于获取令牌的方法返回一个 `AuthenticationResult`（对于异步方法，则会返回 `Task<AuthenticationResult>`）。

在 MSAL.NET 中，`AuthenticationResult` 会公开

- `AccessToken`，以便 Web API 访问资源。 此参数是一个字符串，它通常是一个 base64 编码的 JWT，但客户端不得查看访问令牌的内部信息。 不保证格式稳定，对于资源可将令牌加密。 在客户端上依赖访问令牌内容编写代码是最大的错误来源之一，并且会违反客户端逻辑。 另请参阅[访问令牌](../articles/active-directory/develop/access-tokens.md)
- 适用于用户的 `IdToken`（此参数是编码的 JWT）。 请参阅 [ID 令牌](../articles/active-directory/develop/id-tokens.md)
- `ExpiresOn` 会告知令牌过期的日期/时间
- `TenantId` 包含用户所在的租户。 对于来宾用户（Azure AD B2B 方案），租户 ID 是来宾租户，而不是唯一的租户。
为用户传送令牌时，`AuthenticationResult` 还包含有关此用户的信息。 对于未使用用户帐户请求令牌的机密客户端流（适用于应用程序），此用户信息为 null。
- 令牌的颁发`Scopes`。
- 用户的唯一 ID。

### <a name="iaccount"></a>IAccount

MSAL.NET 定义了帐户的概念（通过 `IAccount` 接口）。 这项中断性变更提供了正确的语义：同一用户可以在不同的 Azure AD 目录中拥有多个帐户这一事实。 此外，由于系统会提供主帐户信息，MSAL.NET 可以在使用来宾方案的情况下提供更有用的信息。
下图显示了 `IAccount` 接口的结构：

![图像](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

`AccountId` 类标识特定租户中的帐户。 它具有以下属性：

| 属性 | 说明 |
|----------|-------------|
| `TenantId` | GUID 的字符串表示形式，是帐户所在租户的 ID。 |
| `ObjectId` | GUID 的字符串表示形式，是拥有租户中的帐户的用户的 ID。 |
| `Identifier` | 帐户的唯一标识符。 `Identifier` 是 `ObjectId` 和 `TenantId`（由逗号分隔，不是 base64 编码）的串联。 |

`IAccount` 接口表示单个帐户的相关信息。 同一用户可以存在于不同的租户中，也就是说，一个用户可以有多个帐户。 其成员为：

| 属性 | 说明 |
|----------|-------------|
| `Username` | 一个字符串，包含 UserPrincipalName (UPN) 格式的可显示值，例如 john.doe@contoso.com。 此字符串可以为 null，而 HomeAccountId 和 HomeAccountId.Identifier 不会是 null。 此属性替换 MSAL.NET 旧版本中 `IUser` 的 `DisplayableId` 属性。 |
| `Environment` | 一个字符串，包含此帐户的标识提供者，例如 `login.partner.microsoftonline.cn`。 此属性替换 `IUser` 的 `IdentityProvider` 属性，不同之处是 `IdentityProvider` 还有除云环境以外的租户信息，而此处的值仅仅是主机。 |
| `HomeAccountId` | 用户的主帐户的 AccountId。 此属性唯一标识 Azure AD 租户的用户。 |

### <a name="using-the-token-to-call-a-protected-api"></a>使用令牌调用受保护的 API

MSAL 返回 `AuthenticationResult` 后（在 `result` 中），需将其添加到 HTTP 授权标头，然后通过调用来访问受保护的 Web API。

```csharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```

