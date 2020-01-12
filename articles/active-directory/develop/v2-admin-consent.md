---
title: Microsoft 标识平台管理员同意协议 | Microsoft Docs
description: 介绍 Microsoft 标识平台终结点中的授权，包括范围、权限和许可。
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 8f98cbf0-a71d-4e34-babf-e642ad9ff423
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: v-junlch
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d155908752e62fbfa4620512c56fe7350338e03
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75777028"
---
# <a name="admin-consent-on-the-microsoft-identity-platform"></a>Microsoft 标识平台中的管理员同意

某些权限需要获得管理员同意才能在租户中授予。  你还可以使用管理员许可终结点向整个租户授予权限。  

## <a name="recommended-sign-the-user-into-your-app"></a>建议：让用户登录到应用

在构建使用管理员许可终结点的应用程序时，应用通常需要一个页面或视图，使管理员能够批准应用的权限。 此页面可以是应用注册流的一部分、应用设置的一部分，或者专用的“连接”流。 在许多情况下，合理的结果是只有在用户使用工作或学校帐户登录后，应用才显示此“连接”视图。

将用户登录到应用后，便可识别管理员所属的组织，然后要求他们批准必要的权限。 尽管在严格意义上不需要这样做，但这有助于为组织用户带来更直观的体验。 若要将用户登录，请遵循 [Microsoft 标识平台协议教程](active-directory-v2-protocols.md)。

## <a name="request-the-permissions-from-a-directory-admin"></a>向目录管理员请求权限

准备好向组织管理员请求权限时，可将用户重定向到 Microsoft 标识平台*管理员许可终结点*。

```
// Line breaks are for legibility only.
    GET https://login.partner.microsoftonline.cn/{tenant}/v2.0/adminconsent?
  client_id=6731de76-14a6-49ae-97bc-6eba6914391e
  &state=12345
  &redirect_uri=http://localhost/myapp/permissions
    &scope=
    https://microsoftgraph.chinacloudapi.cn/calendars.read 
    https://microsoftgraph.chinacloudapi.cn/mail.send
```


| 参数     | 条件     | 说明                                                                               |
|--------------:|--------------:|:-----------------------------------------------------------------------------------------:|
| `tenant` | 必须 | 要向其请求权限的目录租户。 可以采用 GUID 或友好名称格式提供或使用 `organizations` 以一般方式引用，如示例所示。 
| `client_id` | 必须 | [Azure 门户 - 应用注册](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)体验分配给应用的**应用程序（客户端）ID**。 |
| `redirect_uri` | 必须 |要向其发送响应，供应用处理的重定向 URI。 必须与在应用注册门户中注册的重定向 URI 之一完全匹配。 |
| `state` | 建议 | 同样随令牌响应返回的请求中所包含的值。 可以是所需的任何内容的字符串。 使用该状态可在身份验证请求出现之前，在应用中编码用户的状态信息，例如用户过去所在的页面或视图。 |
|`scope`        | 必须      | 定义应用程序请求的权限集。 这可以是静态范围（使用 /.default）或动态范围。  这可以包括 OIDC 范围（`openid`、`profile`、`email`）。 | 


此时，Azure AD 要求租户管理员登录，以完成请求。 系统要求管理员批准你在 `scope` 参数中请求的所有权限。  如果你使用了静态 (`/.default`) 值，则其功能将类似于 v1.0 管理员许可终结点，并请求对应用所需权限中找到的所有范围的许可。

### <a name="successful-response"></a>成功的响应

如果管理员批准了应用的权限，成功响应如下所示：

```
http://localhost/myapp/permissions?admin_consent=True&tenant=fa00d692-e9c7-4460-a743-29f2956fd429&state=12345&scope=https%3a%2f%2fmicrosoftgraph.chinacloudapi.cn%2fCalendars.Read+https%3a%2f%2fmicrosoftgraph.chinacloudapi.cn%2fMail.Send
```

| 参数         | 说明                                                                                       |
|------------------:|:-------------------------------------------------------------------------------------------------:|
| `tenant`| 向应用程序授予所请求权限的目录租户（采用 GUID 格式）。|
| `state`           | 同样随令牌响应返回的请求中所包含的值。 可以是所需的任何内容的字符串。 该 state 用于在身份验证请求出现之前，于应用中编码用户的状态信息，例如之前所在的页面或视图。|
| `scope`          | 为应用程序授予访问权限的权限集。|
| `admin_consent`   | 将设置为 `True`。|

### <a name="error-response"></a>错误响应

`http://localhost/myapp/permissions?error=consent_required&error_description=AADSTS65004%3a+The+resource+owner+or+authorization+server+denied+the+request.%0d%0aTrace+ID%3a+d320620c-3d56-42bc-bc45-4cdd85c41f00%0d%0aCorrelation+ID%3a+8478d534-5b2c-4325-8c2c-51395c342c89%0d%0aTimestamp%3a+2019-09-24+18%3a34%3a26Z&admin_consent=True&tenant=fa15d692-e9c7-4460-a743-29f2956fd429&state=12345`

除了在成功响应中看到的参数外，错误参数如下所示。

| 参数          | 说明                                                                                      |
|-------------------:|:-------------------------------------------------------------------------------------------------:|
| `error`            | 可用于分类发生的错误类型与响应错误的错误码字符串。|
| `error_description`| 可帮助开发人员识别错误根本原因的具体错误消息。|
| `tenant`| 向应用程序授予所请求权限的目录租户（采用 GUID 格式）。|
| `state`           | 同样随令牌响应返回的请求中所包含的值。 可以是所需的任何内容的字符串。 该 state 用于在身份验证请求出现之前，于应用中编码用户的状态信息，例如之前所在的页面或视图。|
| `admin_consent`   | 将设置为 `True`，以指示此响应发生在管理员同意流上。|

## <a name="next-steps"></a>后续步骤
- 请参阅[如何将应用转换为多租户应用](howto-convert-app-to-be-multi-tenant.md)
- 了解如何[在授权代码授予流期间在 OAuth 2.0 协议层提供许可支持](v2-oauth2-auth-code-flow.md#request-an-authorization-code)。
- 了解[多租户应用程序如何使用许可框架](active-directory-devhowto-multi-tenant-overview.md)来实现“用户”许可和“管理员”许可，为更高级的多层应用程序模式提供支持。
- 了解 [Azure AD 应用程序许可体验](application-consent-experience.md)

<!-- Update_Description: wording update -->