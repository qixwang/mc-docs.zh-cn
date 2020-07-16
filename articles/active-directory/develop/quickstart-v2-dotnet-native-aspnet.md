---
title: 调用受 Microsoft 标识平台保护的 ASP.NET Web API
description: 本快速入门介绍如何从 Windows 桌面 (WPF) 应用程序调用受 Microsoft 标识平台保护的 ASP.NET Web API。 WPF 客户端对用户进行身份验证，请求访问令牌，并调用 Web API。
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 07/08/2020
ms.author: v-junlch
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.openlocfilehash: 0da5d0e44979c91b74ca25569119f5ce1fea2bef
ms.sourcegitcommit: 92b9b1387314b60661f5f62db4451c9ff2c49500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86164946"
---
# <a name="quickstart-call-an-aspnet-web-api-protected-by-microsoft-identity-platform"></a>快速入门：调用受 Microsoft 标识平台保护的 ASP.NET Web API

在本快速入门中，你将公开一个 Web API 并对其进行保护，以便只有经过身份验证的用户可以访问它。 本示例演示如何公开 ASP.NET Web API，使它可以接受与 Microsoft 标识平台集成的任何公司或组织中的工作和学校帐户颁发的令牌。

本示例还包含 Windows 桌面应用程序 (WPF) 客户端，该客户端演示了如何请求访问令牌以访问 Web API。

## <a name="prerequisites"></a>先决条件

若要运行本示例，需要满足以下先决条件：

* Visual Studio 2017 或 2019。  免费下载 [Visual Studio](https://www.visualstudio.com/downloads/)。

* [Microsoft 365 开发人员计划](https://docs.microsoft.com/office/developer-program/office-365-developer-program)

## <a name="download-or-clone-this-sample"></a>下载或克隆本示例

可以从 shell 或命令行克隆本示例：

  ```console
  git clone https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git
  ```

或者，可以[下载 ZIP 文件格式的示例](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip)。

## <a name="register-your-web-api-in-the-azure-portal"></a>在 Azure 门户中注册 Web API

### <a name="choose-the-azure-ad-tenant-where-you-want-to-create-your-applications"></a>选择要在其中创建应用程序的 Azure AD 租户

若要手动注册应用，首先需要执行以下步骤：

1. 使用工作或学校帐户登录到 [Azure 门户](https://portal.azure.cn)。
1. 如果你的帐户存在于多个 Azure AD 租户中，请在页面顶部菜单的右上角选择你的个人资料，然后**切换目录**。
   将门户会话更改为所需的 Azure AD 租户。

### <a name="register-the-service-app-todolistservice"></a>注册服务应用 (TodoListService)

1. 导航到面向开发人员的 Microsoft 标识平台的[应用注册](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)页。
1. 选择“新注册”。
1. 出现“注册应用程序”页后，请输入应用程序的注册信息：
   - 在“名称”部分输入一个会显示给应用用户的有意义的应用程序名称，例如 `AppModelv2-NativeClient-DotNet-TodoListService`。
   - 将“支持的帐户类型”更改为“任何组织目录中的帐户”。 
   - 选择“注册”以创建应用程序。

1. 在应用的“概述”页上，找到“应用程序(客户端) ID”值，并记下该值以供后续使用 。 稍后需要使用它为此项目配置 Visual Studio 配置文件（`TodoListService\Web.config` 中的 `ClientId`）。
1. 选择“公开 API”部分并执行以下操作：
   - 选择“添加范围”
   - 选择“保存并继续”，接受建议的应用程序 ID URI (api://{clientId})
   - 输入以下参数：
     - 对于“范围名称”，请使用 `access_as_user`
     - 确保为“谁能许可”选择了“管理员和用户”选项 。
     - 在“管理员许可显示名称”中键入 `Access TodoListService as a user`
     - 在“管理员许可说明”中键入 `Accesses the TodoListService web API as a user`
     - 在“用户许可显示名称”中键入 `Access TodoListService as a user`
     - 在“用户许可说明”中键入 `Accesses the TodoListService web API as a user`
     - 将“状态”保留为“已启用” 
     - 选择“添加范围”

### <a name="configure-the-service-project-to-match-the-registered-web-api"></a>配置服务项目以匹配注册的 Web API

1. 在 Visual Studio 中打开解决方案，然后打开 **TodoListService** 项目根目录下的 **Web.config** 文件。
1. 将 `ida:ClientId` 参数的值替换为刚刚在 Azure 门户中注册的应用程序的“客户端 ID (应用程序 ID)”。

### <a name="add-the-new-scope-to-the-todolistclients-appconfig"></a>将新范围添加到 *TodoListClient* 的 app.config

1. 打开 **TodoListClient** 项目根文件夹中的 **app.config** 文件，然后粘贴刚刚在 `TodoListServiceScope` 参数下为 *TodoListService* 注册的应用程序中的“应用程序 ID”，并替换字符串 `{Enter the Application ID of your TodoListService from the app registration portal}`。

   > 注意：请确保此 ID 使用以下格式：
   >
   > `api://{TodoListService-Application-ID}/access_as_user`
   >
   >（其中，{TodoListService-Application-ID} 是表示 TodoListService 的应用程序 ID 的 GUID）。

## <a name="register-the-client-app-todolistclient"></a>注册客户端应用 (TodoListClient)

此步骤通过在 Azure 门户中注册新应用程序来配置 *TodoListClient* 项目。 如果客户端和服务器被视为同一个应用程序，则可以重复使用“步骤 2”中注册的同一应用程序。 

### <a name="register-the-todolistclient-application-in-the-azure-portal"></a>在 *Azure 门户*中注册 *TodoListClient* 应用程序

1. 导航到面向开发人员的 Microsoft 标识平台的[应用注册](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)页。
1. 选择“新注册”。
1. 出现“注册应用程序”页后，请输入应用程序的注册信息：
   - 在“名称”部分输入一个会显示给应用用户的有意义的应用程序名称，例如 `NativeClient-DotNet-TodoListClient`。
   - 将“支持的帐户类型”更改为“任何组织目录中的帐户”。 
   - 选择“注册”以创建应用程序。
1. 在应用的“概述”页中，选择“身份验证”部分。
   - 在“重定向 URI” | “建议用于公共客户端(移动、桌面)的重定向 URI”部分中，选中 https://login.partner.microsoftonline.cn/common/oauth2/nativeclient  
   - 选择“保存” 。
1. 选择“API 权限”部分
   - 单击“添加权限”按钮，然后
   - 选择“我的 API”选项卡。
   - 在 API 列表中，选择 `AppModelv2-NativeClient-DotNet-TodoListService API` 或你为 Web API 输入的名称。
   - 如果未选中 **access_as_user** 权限，请将其选中。 如有必要，请使用搜索框。
   - 选择“添加权限”按钮

### <a name="configure-your-todolistclient-project"></a>配置 *TodoListClient* 项目

1. 在 *Azure 门户*中的“概述”页上，复制“应用程序(客户端) ID”的值 
1. 打开 **TodoListClient** 项目根文件夹中的 **app.config** 文件，然后将该值粘贴到 `ida:ClientId` 参数值中

## <a name="run-your-project"></a>运行项目

1. 按 `<F5>` 运行项目。 *TodoListClient* 应会打开。
1. 在右上角选择“登录”，并使用注册应用程序时所用的同一用户或者同一目录中的用户身份登录。
1. 此时，如果你是首次登录，系统可能会提示你向 *TodoListService* Web API 许可权限。
1. 登录过程还会请求 *access_as_user* 范围的访问令牌，以便能够访问 *TodoListService* Web API 和处理“待办事项”列表。

## <a name="pre-authorize-your-client-application"></a>为客户端应用程序预先授权

允许其他目录中的用户访问 Web API 的方法之一是：在 Web API 的预授权应用程序列表中添加来自客户端应用程序的应用程序 ID，从而预先授予客户端应用程序访问 Web API 的权限 。 通过添加预授权的客户端，无需用户同意即可使用 Web API。 遵循以下步骤为 Web 应用程序预先授权：

1. 返回 *Azure 门户*并打开 **TodoListService** 的属性。
1. 在“公开 API”部分，单击“已授权的客户端应用程序”部分下的“添加客户端应用程序”。 
1. 在“客户端 ID”字段中，粘贴 `TodoListClient` 应用程序的应用程序 ID。
1. 在“授权范围”部分中，为此 Web API `api://<Application ID>/access_as_user` 选择范围。
1. 按页面底部的“添加应用程序”按钮。

## <a name="run-your-project"></a>运行项目

1. 按 `<F5>` 运行项目。 *TodoListClient* 应会打开。
1. 选择右上角的“登录”（或“清除缓存”/“登录”），然后使用工作或学校帐户登录。

## <a name="optional-restrict-sign-in-access-to-your-application"></a>可选：限制应用程序的登录访问权限

默认情况下，在下载本代码示例并遵循上述步骤将应用程序配置为使用 Azure Active Directory v2 终结点时，与 Azure AD 集成的任何组织中的工作或学校帐户都可以请求令牌并访问你的 Web API。 

若要限制谁可以登录到应用程序，请使用以下选项之一：

### <a name="option-1-restrict-access-to-a-single-organization-single-tenant"></a>选项 1：将访问权限限制为单个组织（单租户）

可将应用程序的登录访问权限限制为单个 Azure AD 租户中的用户帐户 - 包括该租户的来宾帐户。 此方案在业务线应用程序中很常见：

1. 打开 **App_Start \Startup.Auth** 文件，并将传入 `OpenIdConnectSecurityTokenProvider` 的元数据终结点值更改为 `"https://login.partner.microsoftonline.cn/{Tenant ID}/v2.0/.well-known/openid-configuration"`（也可以使用租户名称，例如 `contoso.partner.onmschina.cn`）。
2. 在同一文件中，将 `TokenValidationParameters` 中的 `ValidIssuer` 属性设置为 `"https://sts.chinacloudapi.cn/{Tenant ID}/"`，并将 `ValidateIssuer` 参数设置为 `true`。

### <a name="option-2-use-a-custom-method-to-validate-issuers"></a>选项 2：使用自定义方法来验证颁发者

可通过 IssuerValidator 参数实现自定义方法来验证颁发者。 有关如何使用此参数的详细信息，请阅读 [TokenValidationParameters 类](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters?view=azure-dotnet)。

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>后续步骤
详细了解 Microsoft 标识平台支持的受保护 Web API 方案：
> [!div class="nextstepaction"]
> [受保护的 Web API 方案](scenario-protected-web-api-overview.md)

