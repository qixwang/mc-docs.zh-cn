---
title: Xamarin iOS 注意事项 (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: 了解将 Xamarin iOS 与适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 配合使用时的具体注意事项。
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/06/2020
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a4a0a5e8deb33c4a1488bc183252b6b6e369396
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75776978"
---
# <a name="xamarin-ios-specific-considerations-with-msalnet"></a>与 MSAL.NET 配合使用时特定于 Xamarin iOS 的注意事项
在 Xamarin iOS 上使用 MSAL.NET 时必须考虑的几个注意事项

- [iOS 12 和身份验证的已知问题](#known-issues-with-ios-12-and-authentication)
- [重写并实现 `AppDelegate` 中的 `OpenUrl` 函数](#implement-openurl)
- [启用密钥链组](#enable-keychain-access)
- [启用令牌缓存共享](#enable-token-cache-sharing-across-ios-applications)
- [启用密钥链访问](#enable-keychain-access)

## <a name="implement-openurl"></a>实现 OpenUrl

首先需要重写 `FormsApplicationDelegate` 派生类的 `OpenUrl` 方法，并调用 `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs`。

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

此外，需要定义 URL 方案，让应用获取所需的权限来调用另一个应用，对重定向 URL 采用特定的格式，并在 [Azure 门户](https://portal.azure.cn)中注册此重定向 URL

### <a name="enable-keychain-access"></a>启用密钥链访问

若要启用密钥链访问，应用程序必须有密钥链访问组。
可以在创建应用程序时使用 `WithIosKeychainSecurityGroup()` API 来设置密钥链访问组，如下所示：

若要从缓存和单一登录中获益，你需要在所有应用程序中将密钥链访问组设置为相同的值。

使用 MSAL v4.x 的示例如下：
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

除此更改之外，也可以使用以下访问组或你自己的访问组，在 `Entitlements.plist` 文件中启用密钥链访问： 

```xml
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
  </array>
</dict>
```

使用 `WithIosKeychainSecurityGroup()` API 时，MSAL 自动将安全组追加到应用程序的“团队 ID”(AppIdentifierPrefix)  的末尾，因为当你使用 xcode 生成应用程序时，它也会这样做。 有关详细信息，请参阅 [iOS 权利文档](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps)。 这就是权利需要在 `Entitlements.plist` 中的密钥链访问组之前包含 `$(AppIdentifierPrefix)` 的原因。

### <a name="enable-token-cache-sharing-across-ios-applications"></a>启用 iOS 应用程序之间的令牌缓存共享

从 MSAL 2.x 开始，可以指定一个密钥链访问组，用于在多个应用程序之间保留令牌缓存。 此设置允许在使用相同密钥链访问组的多个应用程序之间共享令牌缓存，其中包括使用 [ADAL.NET](https://aka.ms/adal-net) 开发的应用程序、MSAL.NET Xamarin.iOS 应用程序，以及使用 [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) 或 [MSAL.objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc) 开发的本机 iOS 应用程序。

共享令牌缓存可以在使用相同密钥链访问组的所有应用程序之间实现单一登录。

若要启用此缓存共享，需在所有共享同一缓存的应用程序中使用“WithIosKeychainSecurityGroup()”方法将密钥链访问组设置为同一值，如以上示例所示。

此前提到过，只要使用 `WithIosKeychainSecurityGroup()` API，MSAL 就会添加 $(AppIdentifierPrefix)。 这是因为，AppIdentifierPrefix 或“团队 ID”用于确保只有同一发布者创建的应用程序可以共享密钥链访问权限。

> [!NOTE]
> **`KeychainSecurityGroup` 属性已弃用。**
> 
> 以前，从 MSAL 2.x 开始，开发人员在使用 `KeychainSecurityGroup` 属性时必须包含 TeamId 前缀。
>
>  从 MSAL 2.7.x 开始，在使用新的 `iOSKeychainSecurityGroup` 属性时，MSAL 会在运行时过程中解析 TeamId 前缀。 使用此属性时，其值不应包含 TeamId 前缀。
>  请使用新的 `iOSKeychainSecurityGroup` 属性，你无需在其中提供 TeamId，因为以前的 `KeychainSecurityGroup` 属性现已过时。

### <a name="use-microsoft-authenticator"></a>使用 Microsoft Authenticator

应用程序可以使用 Microsoft Authenticator（中转站）来启用：

- 单一登录 (SSO)。 用户将无需登录到每个应用程序。
- 设备标识。 通过访问设备证书，该证书是在设备加入工作区时在设备上创建的。 
- 应用程序标识验证。 当应用程序调用中介时，它会传递其重定向 URL，而中介会验证该 URL。

有关如何启用中转站的详细信息，请参阅[在 Xamarin iOS 和 Android 应用程序上使用 Microsoft Authenticator 或 Microsoft Intune 公司门户](msal-net-use-brokers-with-xamarin-apps.md)。

### <a name="sample-illustrating-xamarin-ios-specific-properties"></a>演示 Xamarin iOS 特定属性的示例

以下示例的 readme.md 文件的[特定于 iOS 的注意事项](https://github.com/azure-samples/active-directory-xamarin-native-v2#ios-specific-considerations)段落中提供了更多详细信息：

示例 | 平台 | 说明
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS、Android、UWP | 一个简单的 Xamarin Forms 应用，演示了如何使用 MSAL 通过 Azure AD V2.0 终结点对 MSA 和 Azure AD 进行身份验证，以及如何使用生成的令牌访问 Microsoft Graph。

<!--- https://github.com/Azure-Samples/active-directory-xamarin-native-v2/blob/master/ReadmeFiles/Topology.png -->

## <a name="known-issues-with-ios-12-and-authentication"></a>iOS 12 和身份验证的已知问题
Microsoft 已发布[安全公告](https://github.com/aspnet/AspNetCore/issues/4647)，其中提供了有关 iOS12 与某些身份验证类型之间的不兼容性的信息。 这种不兼容性会中断社交、WSFed 和 OIDC 登录。 此公告还提供了相关指导，让开发人员采取措施来消除 ASP.NET 当前在其应用程序中施加的安全限制，以便与 iOS12 兼容。  

在 Xamarin iOS 上开发 MSAL.NET 应用程序的过程中，尝试从 iOS 12 登录到网站时，你可能会看到无限循环（类似于此 [ADAL 问题](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329)）。 

此外，你还可能会看到 iOS 12 Safari 中发生 ASP.NET Core OIDC 身份验证中断，如此 [WebKit 问题](https://bugs.webkit.org/show_bug.cgi?id=188165)中所述。

<!-- Update_Description: wording update -->