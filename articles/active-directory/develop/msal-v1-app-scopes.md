---
title: v1.0 应用的范围 (MSAL) | Azure
description: 了解使用 Microsoft 身份验证库 (MSAL) 的 v1.0 应用程序的范围。
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/22/2020
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 94a696e0d8c94b03772bde70b557f06a5396aa7e
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126496"
---
# <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>接受 v1.0 令牌中的 Web API 的范围

OAuth2 权限是适用于开发人员的 Azure Active Directory (Azure AD) (v1.0) Web API（资源）应用程序向客户端应用程序公开的权限范围。 在许可期间，可将这些权限范围授予客户端应用程序。 请参阅 [Azure Active Directory 应用程序清单参考](reference-app-manifest.md#manifest-reference)中有关 `oauth2Permissions` 的部分。

## <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>将请求访问权限范围限定为 v1.0 应用程序的特定 OAuth2 权限

若要获取 v1.0 应用程序（例如 Microsoft Graph API，网址为 https://microsoftgraph.chinacloudapi.cn) 的特定范围的令牌，请将所需资源标识符与该资源所需的 OAuth2 权限进行连接以创建范围。

例如，若要以用户的身份访问应用 ID URI 为 `ResourceId` 的 v1.0 Web API，请执行以下操作：

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

```javascript
var scopes = [ ResourceId + "/user_impersonation"];
```

若要使用 Microsoft Graph API (https:\//microsoftgraph.chinacloudapi.cn/) 通过 MSAL.NET Azure AD 进行读取和写入，需按以下示例所示创建范围列表：

```csharp
string ResourceId = "https://microsoftgraph.chinacloudapi.cn/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

```javascript
var ResourceId = "https://microsoftgraph.chinacloudapi.cn/";
var scopes = [ ResourceId + "Directory.Read", ResourceID + "Directory.Write"];
```

若要写入对应于 Azure 资源管理器 API (https:\//management.core.chinacloudapi.cn/) 的范围，需要请求以下范围（请注意有两个斜杠）：

```csharp
var scopes = new[] {"https://management.core.chinacloudapi.cn//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.chinacloudapi.cn/subscriptions?api-version=2016-09-01
```

> [!NOTE]
> 你需要使用两个斜杠，这是因为，Azure 资源管理器 API 要求在其受众声明 (aud) 中使用一个斜杠，然后使用一个斜杠来分隔 API 名称与范围。

下面是 Azure AD 使用的逻辑：

- 对于使用 v1.0 访问令牌（只能使用此类令牌）的 ADAL (Azure AD v1.0) 终结点，aud=resource
- 对于要求资源访问令牌接受 v2.0 令牌的 MSAL（Microsoft 标识平台 (v2.0)）终结点，`aud=resource.AppId`
- 对于要求资源访问令牌接受 v1.0 令牌的 MSAL（v2.0 终结点）（与上面的情况相同），Azure AD 将提取最后一个斜杠前面的所有内容并将其用作资源标识符，以分析请求的范围中的所需受众。 因此，如果 https:\//database.chinacloudapi.cn 预期的受众为“https:\//database.chinacloudapi.cn/”，则需要请求的范围为“https:\//database.chinacloudapi.cn//.default”。 另请参阅 GitHub 问题 [#747：将省略资源 URL 的尾部斜杠，因为该斜杠会导致 SQL 身份验证失败](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747)。

## <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>将请求访问权限范围限定为 v1.0 应用程序的所有权限

例如，若要获取 v1.0 应用程序的所有静态范围的令牌，请将“.default”追加到 API 的应用 ID URI：

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

```javascript
var ResourceId = "someAppIDURI";
var scopes = [ ResourceId + "/.default"];
```

## <a name="scopes-to-request-for-a-client-credential-flowdaemon-app"></a>针对客户端凭据流/守护程序应用的请求的范围

使用客户端凭据流时，要传递的范围也是 `/.default`。 这会让 Azure AD 知道管理员在应用程序注册中许可的所有应用级权限。

