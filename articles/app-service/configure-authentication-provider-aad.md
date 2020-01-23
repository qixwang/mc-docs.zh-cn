---
title: 配置 Azure AD 身份验证
description: 了解如何将 Azure Active Directory 身份验证配置为应用服务应用的标识提供者。
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
origin.date: 09/03/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: fddf94ec966e4a7212d4af05832ab4d3a01c4761
ms.sourcegitcommit: cebee33429c25996658d322d337dd05ad1439f89
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75600502"
---
# <a name="configure-your-app-service-app-to-use-azure-ad-login"></a>将应用服务应用配置为使用 Azure AD 登录

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

本文介绍如何将 Azure 应用服务配置为使用 Azure Active Directory (Azure AD) 作为身份验证提供程序。

> [!NOTE]
> 目前，只有 Azure AD v1.0 支持 Azure 应用服务和 Azure Functions。 [Microsoft 标识平台 v2.0](/active-directory/develop/v2-overview)（包括 Microsoft 身份验证库 (MSAL)）不支持这些服务。

设置应用和身份验证时，请遵循以下最佳做法：

- 为每个应用服务应用提供其自身的权限和许可。
- 为每个应用服务应用配置其自身的注册。
- 避免通过对不同的部署槽使用不同的应用注册，在环境之间共享权限。 测试新代码时，这种做法有助于防止问题影响到生产应用。

## <a name="express"> </a>使用快速设置进行配置

1. 在 [Azure 门户]中，转到你的应用服务应用。
1. 在左窗格中选择“设置”   > “身份验证/授权”  ，并确保“应用服务身份验证”  设置为“打开”  。
1. 选择“Azure Active Directory”，然后选择“管理模式”下的“快速”    。
1. 选择“确定”，在 Azure Active Directory 中注册应用服务应用  。 随即会创建一个新的应用注册。

   若要改为选择现有的应用注册：

   1. 选择“选择现有应用”，并在租户中搜索以前创建的应用注册的名称  。
   1. 选择该应用注册，然后选择“确定”。 
   1. 在 Azure Active Directory 设置页上选择“确定”  。

   默认情况下，应用服务提供身份验证但不限制对站点内容和 API 的已授权访问。 必须在应用代码中为用户授权。
1. （可选）若要限制只有通过 Azure Active Directory 身份验证的用户可以访问应用，请将“请求未经身份验证时需执行的操作”  设置为“使用 Azure Active Directory 登录”  。 设置此功能时，应用会要求对所有请求进行身份验证。 它还将所有未经身份验证的用户重定向到 Azure Active Directory 进行身份验证。

    > [!CAUTION]
    > 以这种方式限制访问适用于对应用的所有调用，对于主页公开可用的应用程序来说，这可能是不可取的，就像在许多单页应用程序中一样。 对于此类应用程序，“允许匿名请求(无操作)”  可能是首选，应用本身手动启动登录。 有关详细信息，请参阅[身份验证流](overview-authentication-authorization.md#authentication-flow)。
1. 选择“保存”  。

## <a name="advanced"> </a>使用高级设置进行配置

若要使用一个与用于登录 Azure 的租户不同的 Azure AD 租户，可以手动配置应用设置。 若要完成此自定义配置，需要：

1. 在 Azure AD 中创建一个注册。
1. 向应用服务提供一些注册详细信息。

### <a name="register"> </a>在 Azure AD 中为应用服务应用创建应用注册

配置应用服务应用时，需要提供以下信息：

- 客户端 ID
- 租户 ID
- 客户端机密（可选）
- 应用程序 ID URI

执行以下步骤：

1. 登录到 [Azure 门户]，并转到你的应用服务应用。 记下应用的“URL”。  稍后要使用此 URL 来配置 Azure Active Directory 应用注册。
1. 选择“Azure Active Directory” > “应用注册” > “新建注册”。   
1. 在“注册应用程序”页中，输入应用注册的**名称**。 
1. 在“重定向 URI”中选择“Web”，输入应用服务应用的 URL，并追加路径 `/.auth/login/aad/callback`。   例如，`https://contoso.chinacloudsites.cn/.auth/login/aad/callback`。 
1. 选择“创建”  。
1. 创建应用注册后，复制“应用程序(客户端) ID”和“目录(租户) ID”供稍后使用。  
1. 选择“品牌”。  在“主页 URL”中，输入应用服务应用的 URL，然后选择“保存”。  
1. 选择“公开 API” > “设置”。   粘贴应用服务应用的 URL，然后选择“保存”。 

   > [!NOTE]
   > 此值是应用注册的“应用程序 ID URI”。  如果 Web 应用需要访问云中的 API，则在配置云应用服务资源时，需要提供该 Web 应用的“应用程序 ID URI”。  例如，如果你希望云服务显式向该 Web 应用授予访问权限，则可以使用此 URI。

1. 选择“添加范围”。 
   1. 在“范围名称”中输入 *user_impersonation*。 
   1. 在文本框中，输入许可范围名称，以及希望在许可页上向用户显示的说明。 例如，输入“访问我的应用”。  
   1. 选择“添加作用域”。 
1. （可选）若要创建客户端机密，请选择“证书和机密” > “新建客户端机密” > “添加”。    复制页面中显示的客户端机密值。 它不会再次显示。
1. （可选）若要添加多个“回复 URL”，请选择“身份验证”。  

### <a name="secrets"> </a>将 Azure Active Directory 信息添加到应用服务应用

1. 在 [Azure 门户]中，转到你的应用服务应用。 
1. 在左窗格中选择“设置”>“身份验证/授权”，并确保“应用服务身份验证”设置为“打开”。   
1. （可选）默认情况下，应用服务身份验证允许在未经过身份验证的情况下访问应用。 若要强制用户身份验证，请将“请求未经身份验证时需执行的操作”设置为“使用 Azure Active Directory 登录”   。
1. 在“身份验证提供程序”下，选择“Azure Active Directory”  。
1. 在“管理模式”中，选择“高级”并根据下表配置应用服务身份验证：  

    |字段|说明|
    |-|-|
    |客户端 ID| 使用应用注册的“应用程序(客户端) ID”。  |
    |颁发者 ID| 使用 `https://login.partner.microsoftonline.cn/<tenant-id>`，并将 *\<tenant-id>* 替换为应用注册的“目录(租户) ID”。  |
    |客户端机密（可选）| 使用在应用注册中生成的客户端机密。|
    |允许的令牌受众| 如果这是一个云应用或服务器应用，而你希望允许来自 Web 应用的身份验证令牌，请在此处添加该 Web 应用的“应用程序 ID URI”。  |

    > [!NOTE]
    > 无论“允许的令牌受众”的配置方式如何，都始终会将配置的“客户端 ID”隐式视为允许的受众。   
1. 选择“确定”，然后选择“保存”   。

现在，可以使用 Azure Active Directory 在应用服务应用中进行身份验证。

## <a name="configure-a-native-client-application"></a>配置本机客户端应用程序

可以注册本机客户端，以允许使用 **Active Directory 身份验证库**等客户端库进行身份验证。

1. 在 [Azure 门户]中，选择“Active Directory”   > “应用注册”   > “新建注册”  。
1. 在“注册应用程序”页中，输入应用注册的**名称**。 
1. 在“重定向 URI”中选择“公共客户端(移动和桌面)”，输入应用服务应用的 URL，并追加路径 `/.auth/login/aad/callback`。   例如，`https://contoso.chinacloudsites.cn/.auth/login/aad/callback`。
1. 选择“创建”  。

    > [!NOTE]
    > 对于 Windows 应用程序，请改用[包 SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) 作为 URI。
1. 创建应用注册后，复制“应用程序(客户端) ID”的值。 
1. 选择“API 权限” > “添加权限” > “我的 API”。   
1. 选择前面为应用服务应用创建的应用注册。 如果未看到该应用注册，请确保在[在 Azure AD 中为应用服务应用创建应用注册](#register)部分已添加 **user_impersonation** 范围。
1. 依次选择“user_impersonation”、“添加权限”。  

现已配置可以访问应用服务应用的本机客户端应用程序。

## <a name="related-content"></a>后续步骤

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/configure-authentication-provider-aad/app-service-webapp-auth-config.png



<!-- URLs. -->

[Azure 门户]: https://portal.azure.cn/
[alternative method]:#advanced