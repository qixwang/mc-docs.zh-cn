---
title: Microsoft 标识平台中的身份验证 | Azure
description: 了解 Microsoft 标识平台 (v2.0) 中的身份验证的基础知识。
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/06/2020
ms.author: v-junlch
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c3c42551227b5760a3b4edefed218e8b470b342
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75776840"
---
# <a name="authentication-basics"></a>身份验证基础知识

## <a name="what-is-authentication"></a>什么是身份验证

本文解释在创建受保护的 Web 应用、Web API 或创建调用受保护 Web API 的应用时需要了解的许多身份验证概念。

**身份验证**就是证明自己的身份的过程。 身份验证有时缩写为 AuthN。

**授权**是指授予经过身份验证的一方执行某项操作的权限的措施。 它指定了你可以访问的数据，以及可对该数据执行的操作。 授权有时缩写为 AuthZ。

无需创建每个需要维护自身用户名和密码信息的应用（需要在多个应用中添加或删除用户时，这会产生较高的管理负担），而可以让应用将这种责任委托给一个集中式标识提供者。

Azure Active Directory (Azure AD) 是云中的集中式标识提供者。 向其委托身份验证和授权可以实现要求用户位于特定位置的方案、使用多重身份验证，并可让用户登录一次，然后自动登录到共享同一集中式目录的所有 Web 应用。 此功能称为单一登录 (SSO)。

对于用户位于全球各地，且用户不一定从企业网络登录的应用而言，集中式标识提供者更为重要。 Azure AD 将对用户进行身份验证并提供访问令牌。 访问令牌是授权服务器颁发的安全令牌。 其中包含有关该令牌所针对的用户和应用的信息，可用于访问 Web API 和其他受保护资源。

Microsoft 标识平台通过以下方式简化了对应用程序开发人员的身份验证：将标识提供为一项服务、支持行业标准协议（例如 OAuth 2.0 和 OpenID Connect），并提供用于不同平台的开源库来帮助你快速开始编写代码。 开发人员可以通过它来生成应用程序，以便进行所有 Microsoft 标识的登录，以及获取令牌来调用 Microsoft Graph、其他 Microsoft API 或者开发人员生成的 API。 有关详细信息，请参阅 [Microsoft 标识平台的发展](about-microsoft-identity-platform.md)。

### <a name="tenants"></a>租户

云标识提供者为许多组织提供服务。 为使不同组织中的用户保持隔离，Azure AD 已分区成租户，每个组织有一个租户。

租户跟踪用户及其关联的应用。

Azure AD 还提供 Azure Active Directory B2C，使组织能够使用社交标识将用户（通常是客户）登录。 有关详细信息，请参阅 [Azure Active Directory B2C 文档](/active-directory-b2c)。

### <a name="security-tokens"></a>安全令牌

安全令牌包含有关用户和应用的信息。 Azure AD 使用包含声明的基于 JSON 的令牌 (JWT)。 声明向一个实体提供有关另一个实体的断言。 应用程序可以使用声明来完成各种任务，例如：

* 验证令牌
* 识别使用者的目录租户
* 显示用户信息
* 确定使用者的授权

声明由提供如下信息的键值对组成：

- 生成令牌的安全令牌服务器。
- 令牌生成日期。
- 使用者，例如用户（守护程序除外）。
- 受众，即，为其生成了令牌的应用。
- 请求令牌的应用（客户端）。 对于 Web 应用，这可能与受众相同。

有关声明的更详细信息，请参阅[访问令牌](access-tokens.md)和 [ID 令牌](id-tokens.md)。

由为其生成了令牌的应用、将登录用户的 Web 应用或所调用的 Web API 负责验证令牌。 令牌由安全令牌服务器 (STS) 使用私钥签名。 STS 发布相应的公钥。 若要验证令牌，应用需使用 STS 公钥验证签名，以验证签名是使用私钥创建的。

令牌仅在有限的时间内有效。 通常，STS 会提供一对令牌：一个用于访问应用程序或受保护资源的访问令牌，以及一个在访问令牌即将过期时用于刷新访问令牌的刷新令牌。 

访问令牌作为 `Authorization` 标头中的持有者令牌传递给 Web API。 应用可向 STS 提供刷新令牌，如果用户对应用的访问权限未吊销，则应用将取回新的访问令牌和新的刷新令牌。 用户离职的场景就是这样处理的。 当 STS 收到刷新令牌时，如果用户不再获得授权，则 STS 不会颁发另一个有效的访问令牌。

## <a name="application-model"></a>应用程序模型

应用程序可以自行将用户登录，或者委托标识提供者登录。 有关 Azure AD 支持的登录方案，请参阅[身份验证流和应用方案](authentication-flows-app-scenarios.md)。

要使标识提供者知道某个用户有权访问特定的应用，必须同时将该用户和应用程序注册到标识提供者。 将应用程序注册到 Azure AD 时，需要提供应用程序的标识配置，使其能够与 Azure AD 集成。 注册应用还可以：

- 在登录对话框中自定义应用程序的品牌。 这一点很重要，因为这是用户首次体验你的应用。
- 确定是否只允许属于你的组织的用户登录。 这是一个单租户应用程序。 或者允许用户使用任何工作或学校帐户登录。 这是一个多租户应用程序。 
- 请求范围权限。 例如，可以请求“user.read”范围，该授予读取已登录用户的个人资料的权限。
- 定义范围，用于定义对 Web API 的访问权限。 通常，当某个应用想要访问你的 API 时，它需要请求对你所定义的范围的权限。
- 与 Azure AD 共享机密，以向 Azure AD 证明应用的标识。  这适用于应用是机密客户端应用程序的情况。 机密客户端应用程序是可以安全保存凭据的应用程序。 它们需要使用受信任的后端服务器来存储凭据。

注册后，将为应用程序提供一个 GUID，应用在请求令牌时将与 Azure AD 共享该 GUID。 如果应用是机密客户端应用程序，则它还会根据使用的是证书还是机密，来共享机密或公钥。

Microsoft 标识平台使用实现以下两项主要功能的模型来表示应用程序：

按应用支持的身份验证协议来识别应用，并提供身份验证时所需的所有标识符、URL、机密和相关信息。
Microsoft 标识平台：

* 保存运行时支持身份验证所需的所有数据。
* 保存所有数据，以确定应用可能需要访问的资源，以及在哪些情况下应满足给定的请求。
* 提供用于在应用开发人员的租户和任何其他 Azure AD 租户中实现应用配置的基础设施。
* 在令牌请求期间处理用户同意并帮助跨租户动态预配应用

同意是资源所有者授权客户端应用程序代表资源所有者在特定权限下访问受保护资源的过程。 Microsoft 标识平台：

* 使用户和管理员能够动态地同意或拒绝应用以他们的名义访问资源。
* 使管理员能够最终决定允许执行哪些应用、哪些用户可以使用特定的应用，以及如何访问目录资源。

在 Microsoft 标识平台中，**应用程序对象**将应用程序描述为抽象实体。 在部署时，Microsoft 标识平台使用应用程序对象作为蓝图来创建**服务主体**，它表示目录或租户中的应用程序的具体实例。 该服务主体定义应用在特定目标目录中可以实际执行的操作、使用者是谁、以及可以访问哪些资源等。 Microsoft 标识平台通过**许可**使用应用程序对象创建服务主体。

下图显示了征得同意后经过简化的 Microsoft 标识平台预配流程。 它显示两个租户（A 和 B）。 租户 A 拥有该应用程序。 租户 B 通过服务主体实例化该应用程序。  

![征得同意后经过简化的预配流程](./media/authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

在此预配流程中：

1. 来自租户 B 的某个用户尝试使用该应用登录，授权终结点请求应用程序的令牌。
1. 获取并验证用于身份验证的用户凭据。
1. 系统提示用户许可该应用访问租户 B。
1. Microsoft 标识平台使用租户 A 中的应用程序对象作为在租户 B 中创建服务主体的蓝图。
1. 用户接收请求的令牌。

可对其他租户重复此过程。 租户 A 保留了应用（应用程序对象）的蓝图。 应用获得许可的所有其他租户中的用户和管理员通过每个租户中的相应服务主体对象保留对应用程序允许执行的操作的控制权。 有关详细信息，请参阅 [Microsoft 标识平台中的应用程序和服务主体对象](app-objects-and-service-principals.md)。

## <a name="web-app-sign-in-flow-with-azure-ad"></a>使用 Azure AD 的 Web 应用登录流

当用户在浏览器中导航到某个 Web 应用时，将发生以下情况：

- Web 应用确定用户是否已完成身份验证。
- 如果用户未完成身份验证，Web 应用将委托 Azure AD 将用户登录。 登录符合组织的策略，这可能意味着，系统会要求用户输入其凭据、使用多重身份验证，或者完全禁止使用密码（例如使用 Windows Hello）。
- 系统要求用户许可客户端应用所需的访问权限。 正因如此，需要在 Azure AD 中注册客户端应用，使 Azure AD 能够传送代表用户许可的访问权限的令牌。

用户成功完成身份验证后：

- Azure AD 将令牌发送到 Web 应用。
- 保存与 Azure AD 的域关联的 Cookie，其中包含浏览器 Cookie jar 中的用户标识。 下次应用使用浏览器导航到 Azure AD 授权终结点时，浏览器将提供该 Cookie，因此用户无需再次登录。 这也是实现 SSO 的方式。 Cookie 由 Azure AD 生成，只有 Azure AD 能够识别它。
- 然后，Web 应用验证令牌。 如果验证成功，Web 应用将显示受保护的页，并在浏览器的 Cookie jar 中保存会话 Cookie。 当用户导航到另一页时，Web 应用知道用户已基于会话 Cookie 进行身份验证。

以下序列图汇总了这种交互：

![Web 应用身份验证过程](./media/authentication-scenarios/web-app-how-it-appears-to-be.png)

### <a name="how-a-web-app-determines-if-the-user-is-authenticated"></a>Web 应用如何确定用户是否已完成身份验证

Web 应用开发人员可以指定是所有页还是只有特定的页需要身份验证。 例如，在 ASP.NET/ASP.NET Core 中，可以通过将 `[Authorize]` 属性添加到控制器操作来进行这种指定。 

此属性会导致 ASP.NET 检查是否存在包含用户标识的会话 Cookie。 如果 Cookie 不存在，ASP.NET 会将身份验证重定向到指定的标识提供者。 如果标识提供者是 Azure AD，则 Web 应用会将身份验证重定向到 https://login.partner.microsoftonline.cn ，这会显示登录对话框。

### <a name="how-a-web-app-delegates-sign-in-to-azure-ad-and-obtains-a-token"></a>Web 应用如何将登录委托给 Azure AD 和获取令牌

用户身份验证通过浏览器发生。 OpenID 协议使用标准 HTTP 协议消息。
- Web 应用将 HTTP 302（重定向）发送到浏览器以使用 Azure AD。
- 对用户进行身份验证时，Azure AD 通过浏览器使用重定向将令牌发送到 Web 应用。
- 重定向由 Web 应用以重定向 URI 的形式提供。 此重定向 URI 将注册到 Azure AD 应用程序对象。 由于应用程序可部署到多个 URL，可能存在多个重定向 URI。 因此，Web 应用还需要指定要使用的重定向 URI。
- Azure AD 验证 Web 应用发送的重定向 URI 是否为应用的已注册重定向 URI 之一。

## <a name="desktop-and-mobile-app-sign-in-flow-with-azure-ad"></a>使用 Azure AD 的桌面和移动应用登录流

上面所述的流适用于桌面和移动应用程序，只是存在少许差异。

桌面和移动应用程序可以使用嵌入式 Web 控件或系统浏览器进行身份验证。 下图演示了桌面或移动应用如何使用 Microsoft 身份验证库 (MSAL) 获取访问令牌和调用 Web API。

![桌面应用的身份验证方式](./media/authentication-scenarios/desktop-app-how-it-appears-to-be.png)

MSAL 使用浏览器获取令牌，与对 Web 应用一样，将身份验证委托给 Azure AD。

由于 Azure AD 与对 Web 应用一样在浏览器中保存相同的标识 Cookie，因此，如果本机应用或移动应用使用系统浏览器，则会立即在相应的 Web 应用上实现 SSO。

默认情况下，MSAL 使用系统浏览器，但 .NET Framework 桌面应用程序除外，其中使用嵌入式控件来提供集成程度更高的用户体验。

## <a name="next-steps"></a>后续步骤

- 请参阅 [Microsoft 标识平台开发人员术语表](developer-glossary.md)来熟悉常用术语。
- 请参阅[身份验证流和应用方案](authentication-flows-app-scenarios.md)来详细了解 Microsoft 标识平台支持的其他用户身份验证方案。
- 请参阅 [MSAL 库](msal-overview.md)，了解可以借助哪些 Microsoft 库在单个简化编程模型中开发可以处理 Azure AD 帐户和 Azure AD B2C 用户的应用程序。
- 请参阅[将应用服务与 Microsoft 标识平台集成](/app-service/configure-authentication-provider-aad)，以了解如何为应用服务应用配置身份验证。

<!-- Update_Description: wording update -->
