---
title: Xamarin Android 注意事项 (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: 了解将 Xamarin Android 与适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 配合使用时的具体注意事项。
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
ms.openlocfilehash: 4dcb0c0a37f7bd7775536a0b6bd9a5f9481a58c4
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75776979"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>与 MSAL.NET 配合使用时特定于 Xamarin Android 的注意事项
本文介绍将 Xamarin Android 与适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 配合使用时的具体注意事项。

## <a name="set-the-parent-activity"></a>设置父活动

需在 Xamarin.Android 上设置父活动，以便交互发生后返回令牌。

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```
也可以通过回调在 PublicClientApplication 级别（MSAL4.2+ 中）设置此项。

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => parentActivity)
  .Build();
```

建议使用[此处](https://github.com/jamesmontemagno/CurrentActivityPlugin)的 CurrentActivityPlugin。  然后，PublicClientApplication 生成器代码将如下所示：

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```


## <a name="ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends"></a>确保身份验证流的交互部分结束后控制返回到 MSAL。
在 Android 上，需重写 `Activity` 的 `OnActivityResult` 方法并调用 AuthenticationContinuationHelper MSAL 类的 SetAuthenticationContinuationEventArgs 方法。

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```
该行确保在身份验证流的交互部分结束后控制会返回到 MSAL。

## <a name="update-the-android-manifest"></a>更新 Android 清单
`AndroidManifest.xml` 应包含以下值：
```csharp
<activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="msal{client_id}" android:host="auth" />
         </intent-filter>
</activity>
```

或者，可以[用代码创建活动](https://docs.microsoft.com/xamarin/android/platform/android-manifest#the-basics)，而无需手动编辑`AndroidManifest.xml`。 为此，必须创建一个具有 `Activity` 和 `IntentFilter` 属性的类。 表示上述 xml 的相同值的类为：

```csharp
  [Activity]
  [IntentFilter(new[] { Intent.ActionView },
        Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
        DataHost = "auth",
        DataScheme = "msal{client_id}")]
  public class MsalActivity : BrowserTabActivity
  {
  }
```

### <a name="xamarinforms-43x-manifest"></a>XamarinForms 4.3.X 清单

XamarinForms 4.3.x 生成的代码在 `AndroidManifest.xml` 中将 `package` 属性设置为 `com.companyname.{appName}`。 如果将 `DataScheme` 用作 `msal{client_id}`，则可能需要将该值更改为与 `MainActivity.cs` 命名空间相同。

## <a name="use-the-embedded-web-view-optional"></a>使用嵌入式 Web 视图（可选）

默认情况下，MSAL.NET 使用系统 Web 浏览器，它使你能够通过 Web 应用程序和其他应用获取 SSO。 在某些罕见情况下，可能需要指定你需要使用嵌入式 Web 视图。 有关详细信息，请参阅 [MSAL.NET 使用 Web 浏览器](msal-net-web-browsers.md)和 [Android 系统浏览器](msal-net-system-browser-android-considerations.md)。

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

## <a name="troubleshooting"></a>故障排除
如果创建新的 Xamarin.Forms 应用程序并添加对 MSAL.Net NuGet 包的引用，则这会刚好适用。
但是，如果需要将现有的 Xamarin.Forms 应用程序升级到 MSAL.NET 预览版 1.1.2 或更高版本，则可能会遇到内部版本问题。

若要排查这些问题，应执行以下操作：
- 将现有的 MSAL.NET NuGet 包更新到 MSAL.NET 预览版 1.1.2 或更新版本
- 检查 Xamarin.Forms 是否已自动更新到版本 2.5.0.122203（否则请更新到此版本）
- 检查 Xamarin.Android.Support.v4 是否已自动更新到版本 25.4.0.2（否则请更新到此版本）
- 所有 Xamarin.Android.Support 包应该都以版本 25.4.0.2 为目标
- 清除/重新生成
- 尝试在 Visual Studio 中将最大并行项目内部版本数设置为 1（“选项”->“项目和解决方案”->“生成并运行”->“最大并行项目内部版本数”）
- 或者，如果是从命令行生成的，请尝试从命令中删除“/m”（如果正在使用它）。


### <a name="error-the-name-authenticationcontinuationhelper-does-not-exist-in-the-current-context"></a>错误：名称 'AuthenticationContinuationHelper' 不存在于当前上下文中

这可能是因为 Visual Studio 未正确更新 Android.csproj* 文件。 有时， **\<HintPath>** 文件路径错误地包含 netstandard13 而不是 **monoandroid90**。

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>后续步骤

如需更多详细信息和示例，请参阅以下示例的 readme.md 文件的[特定于 Android 的注意事项](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations)段落：

| 示例 | 平台 | 说明 |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS、Android、UWP | 一个简单的 Xamarin Forms 应用，它展示了如何使用 MSAL 通过 AADD v2.0 终结点对 MSA 和 Azure AD 进行身份验证，以及如何使用生成的令牌访问 Microsoft Graph。 <br>![拓扑](./media/msal-net-xamarin-android-considerations/topology.png) |

<!-- Update_Description: wording update -->