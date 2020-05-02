---
title: Microsoft 标识平台的应用程序类型 | Azure
description: Microsoft 标识平台 (v2.0) 终结点支持的应用类型和方案。
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: v-junlch
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 07d54495cf8a6baf3ac0b68a3a176980db75c91b
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126412"
---
# <a name="application-types-for-microsoft-identity-platform"></a>Microsoft 标识平台的应用程序类型

Microsoft 标识平台 (v2.0) 终结点支持各种现代应用体系结构的身份验证，所有这些体系结构都基于行业标准协议 [OAuth 2.0 或 OpenID Connect](active-directory-v2-protocols.md)。 本文介绍可以使用 Microsoft 标识平台构建的应用类型（不管偏好的语言或平台是什么）。 其中的信息旨在帮助你在[开始处理代码](v2-overview.md#getting-started)之前了解高级方案。

> [!NOTE]
> Microsoft 标识平台终结点并非支持所有 Azure Active Directory (Azure AD) 方案和功能。 若要确定是否应使用 Microsoft 标识平台终结点，请阅读 [Microsoft 标识平台限制](azure-ad-endpoint-comparison.md)。

## <a name="the-basics"></a>基础知识

必须在 [Azure 门户](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)中注册每个使用 Microsoft 标识平台终结点的应用。 应用注册过程收集并分配应用的以下值：

* 用于唯一标识应用的 **应用程序（客户端）ID**
* 用于将响应定向回应用的重定向 URI 
* 几个其他特定于方案的值，例如支持的帐户类型

有关详细信息，请了解如何[注册应用](quickstart-register-app.md)。

注册应用后，应用将通过向终结点发送请求来与 Microsoft 标识平台通信。 我们提供了用于处理这些请求详细信息的开源框架和库。 也可以通过针对这些终结点创建请求，来自行实现身份验证逻辑：

```HTTP
https://login.partner.microsoftonline.cn/common/oauth2/v2.0/authorize
https://login.partner.microsoftonline.cn/common/oauth2/v2.0/token
```

## <a name="single-page-apps-javascript"></a>单页应用 (JavaScript)

许多新式应用都有一个单页应用前端（主要以 JavaScript 编写）。 通常，该前端是使用 Angular、React 或 Vue 等框架编写的。 Microsoft 标识平台终结点使用 [OAuth 2.0 隐式流](v2-oauth2-implicit-grant-flow.md)来支持这些应用。

在此流中，应用直接从 Microsoft 标识平台授权终结点接收令牌，无需任何服务器到服务器的交换。 所有身份验证逻辑和会话处理将完全在 JavaScript 客户端中发生，无需额外的页面重定向。

![显示隐式身份验证流](./media/v2-app-types/convergence-scenarios-implicit.svg)

若要查看此方案的实际运行情况，请尝试运行 [Microsoft 标识平台入门](v2-overview.md#getting-started)部分提供的单页应用代码示例之一。

## <a name="web-apps"></a>Web 应用

对于通过浏览器访问的 Web 应用（.NET、PHP、Java、Ruby、Python、Node 等），可以使用 [OpenID Connect](active-directory-v2-protocols.md) 来执行用户登录。 在 OpenID Connect 中，Web 应用接收一个 ID 令牌。 ID 令牌是一个安全令牌，用于验证用户的标识并以声明形式提供用户相关信息：

```JSON
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

有关 Microsoft 标识平台终结点中使用的不同类型令牌的更多详细信息，请参阅[访问令牌](access-tokens.md)参考和 [id_token 参考](id-tokens.md)

在 Web 服务器应用中，登录身份验证流大致采用以下步骤：

![显示 Web 应用身份验证流](./media/v2-app-types/convergence-scenarios-webapp.svg)

使用从 Microsoft 标识平台终结点接收的公共签名密钥验证 ID 令牌便可确保用户的标识正确。 这会设置一个可在后续页面请求中用于识别用户的会话 Cookie。

若要查看此方案的实际运行情况，请尝试运行 [Microsoft 标识平台入门](v2-overview.md#getting-started)部分提供的 Web 应用登录代码示例之一。

除了简单登录以外，Web 服务器应用可能还需要访问其他 Web 服务，例如 REST API。 在这种情况下，Web 服务器应用使用 [OAuth 2.0 授权代码流](active-directory-v2-protocols.md)参与合并的 OpenID Connect 和 OAuth 2.0 流。 有关此方案的详细信息，请阅读 [Web 应用和 Web API 入门](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)。

## <a name="web-apis"></a>Web API

可以使用 Microsoft 标识平台终结点来保护 Web 服务，例如应用的 RESTful Web API。 Web API 可以在多种平台和语言中实现。 它们还可以在 Azure Functions 中使用 HTTP 触发器实现。 Web API 使用 OAuth 2.0 访问令牌而不是 ID 令牌和会话 Cookie 来保护数据以及对传入的请求进行身份验证。 Web API 调用方会在 HTTP 请求的授权标头中追加一个访问令牌，如下所示：

```HTTP
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API 使用此访问令牌来验证 API 调用方的标识，并从访问令牌中编码的声明提取调用方的相关信息。 有关 Microsoft 标识平台终结点中使用的不同类型令牌的更多详细信息，请参阅[访问令牌](access-tokens.md)参考和 [id_token](id-tokens.md) 参考。

Web API 可让用户通过公开权限（也称为[范围](v2-permissions-and-consent.md)）来选择添加或排除特定功能或数据。 为了使调用应用能够获取某个范围的权限，用户必须在执行流的过程中许可该范围。 Microsoft 标识平台终结点向用户请求权限，并将这些权限记录到 Web API 收到的所有访问令牌中。 Web API 会验证每次调用后收到的访问令牌，并执行授权检查。

Web API 可以从各种应用（包括 Web 服务器应用、桌面和移动应用、单页应用、服务器端守护程序，甚至其他 Web API）接收访问令牌。 Web API 的高级流如下所示：

![显示 Web API 身份验证流](./media/v2-app-types/convergence-scenarios-webapi.svg)

若要了解如何使用 OAuth2 访问令牌保护 Web API，请查看 [Microsoft 标识平台入门](v2-overview.md#getting-started)部分中的 Web API 代码示例。

在许多情况下，Web API 还需要对由 Microsoft 标识平台保护的其他下游 Web API 发出出站请求。 为执行此操作，Web API 可以利用**代理**流，它允许 Web API 将传入的访问令牌替换为将在出站请求中使用的另一个访问令牌。 有关详细信息，请参阅 [Microsoft 标识平台和 OAuth 2.0 代理流](v2-oauth2-on-behalf-of-flow.md)。

## <a name="mobile-and-native-apps"></a>移动和本机应用

设备安装的应用（如移动和桌面应用）通常需要访问用于存储数据和代表用户执行功能的后端服务或 Web API。 这些应用可以使用 [OAuth 2.0 授权代码流](v2-oauth2-auth-code-flow.md)将登录凭据和授权添加到后端服务。

在此流中，应用在用户登录时从 Microsoft 标识平台终结点接收授权代码。 授权代码表示应用有权代表登录用户调用后端服务。 应用可在后台交换 OAuth 2.0 访问令牌和刷新令牌的授权代码。 应用可以在 HTTP 请求中使用访问令牌向 Web API 进行身份验证；在旧的访问令牌过期时，可以使用刷新令牌获取新的访问令牌。

![显示本机应用身份验证流](./media/v2-app-types/convergence-scenarios-native.svg)

## <a name="daemons-and-server-side-apps"></a>守护程序和服务器端应用

具有长时间运行的进程或在不与用户交互的情况下运行的应用也需要一种访问受保护资源的方法，例如 Web API。 这些应用可以通过 OAuth 2.0 客户端凭据流，使用应用的标识（而不是用户的委托标识）来进行身份验证和获取令牌。 可以使用客户端机密或证书证明应用的身份。 有关详细信息，请参阅[使用 Microsoft 标识平台的 .NET Core 守护程序控制台应用程序](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2)。

在此流中，应用通过直接与 `/token` 终结点交互来获取访问权限：

![显示守护程序应用身份验证流](./media/v2-app-types/convergence-scenarios-daemon.svg)

若要生成守护程序，请参阅[客户端凭据文档](v2-oauth2-client-creds-grant-flow.md)，或者尝试 [.NET 示例应用](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)。

