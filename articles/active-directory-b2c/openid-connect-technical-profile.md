---
title: 在自定义策略中定义 OpenID Connect 技术配置文件
titleSuffix: Azure AD B2C
description: 在 Azure Active Directory B2C 的自定义策略中定义 OpenID Connect 技术配置文件。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/16/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 04ae5fca1a9d4a30fa3c37354863a3e3e596a2e8
ms.sourcegitcommit: 71a386ca0d0ecb79a123399b6ab6b8c70ea2aa78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2020
ms.locfileid: "79497185"
---
# <a name="define-an-openid-connect-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>在 Azure Active Directory B2C 自定义策略中定义 OpenID Connect 技术配置文件

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) 为 [OpenID Connect](https://openid.net/2015/04/17/openid-connect-certification-program/) 协议标识提供者提供支持。 OpenID Connect 1.0 在 OAuth 2.0 基础上提供一个标识层，代表现代身份验证协议的最新技术。 使用 OpenID Connect 技术配置文件，可以与基于 OpenID Connect 的标识提供者（如 Azure AD）联合。 与标识提供者联合允许用户使用其现有的社交或企业标识登录。

## <a name="protocol"></a>协议

“Protocol”  元素的“Name”  属性必须设置为 `OpenIdConnect`。 

## <a name="input-claims"></a>输入声明

**InputClaims** 和 **InputClaimsTransformations** 元素不是必需的。 但是，你可能需要向标识提供者发送更多参数。 以下示例将值为 `contoso.com` 的 **domain_hint** 查询字符串参数添加到授权请求。

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>输出声明

**OutputClaims** 元素包含 OpenID Connect 标识提供者返回的声明列表。 可能需要将策略中定义的声明名称映射到标识提供者中定义的名称。 如果设置了 `DefaultValue` 属性，则还可以包含标识提供者不会返回的声明。

**OutputClaimsTransformations** 元素可能包含用于修改输出声明或生成新输出声明的 **OutputClaimsTransformation** 元素集合。

技术配置文件还会返回标识提供者不返回的声明：

- **identityProvider** 声明，其中包含标识提供者的名称。
- **authenticationSource** 声明，其默认值为 **socialIdpAuthentication**。

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
</OutputClaims>
```

## <a name="metadata"></a>Metadata

| 属性 | 必须 | 说明 |
| --------- | -------- | ----------- |
| client_id | 是 | 标识提供者的应用程序标识符。 |
| IdTokenAudience | 否 | id_token 的受众。 如果指定了此项，Azure AD B2C 将检查标识提供者返回的令牌中的 `aud` 声明是否与 IdTokenAudience 元数据中指定的声明相同。  |
| METADATA | 是 | 指向 OpenID Connect 标识提供者配置文档的 URL，也称为 OpenID 众所周知的配置终结点。 URL 可以包含 `{tenant}` 表达式，该表达式将替换为租户名称。  |
| authorization_endpoint | 否 | 指向 OpenID Connect 标识提供者配置授权终结点的 URL。 authorization_endpoint 元数据的值优先于 OpenID 众所周知的配置终结点中指定的 `authorization_endpoint`。 URL 可以包含 `{tenant}` 表达式，该表达式将替换为租户名称。 |
| 颁发者 | 否 | OpenID Connect 标识提供者的唯一标识符。 颁发者元数据的值优先于 OpenID 众所周知的配置终结点中指定的 `issuer`。  如果指定了此项，Azure AD B2C 将检查标识提供者返回的令牌中的 `iss` 声明是否与颁发者元数据中指定的声明相同。 |
| ProviderName | 否 | 标识提供者的名称。  |
| response_types | 否 | 符合 OpenID Connect Core 1.0 规范的响应类型。 可能的值：`id_token`、`code` 或 `token`。 |
| response_mode | 否 | 标识提供者在将结果发送回 Azure AD B2C 时使用的方法。 可能的值：`query`、`form_post`（默认值）或 `fragment`。 |
| scope | 否 | 根据 OpenID Connect Core 1.0 规范定义的请求的范围。 例如 `openid`、`profile` 和 `email`。 |
| HttpBinding | 否 | 预期的 HTTP 绑定，绑定到访问令牌和声明令牌终结点。 可能的值：`GET` 或 `POST`。  |
| ValidTokenIssuerPrefixes | 否 | 一个密钥，可以在使用多租户标识提供者（例如 Azure Active Directory）时用于登录到每个租户。 |
| UsePolicyInRedirectUri | 否 | 指示在构造重定向 URI 时是否要使用策略。 在标识提供者中配置应用程序时，需指定重定向 URI。 重定向 URI 指向 Azure AD B2C `https://{your-tenant-name}.b2clogin.cn/{your-tenant-name}.partner.onmschina.cn/oauth2/authresp`。  如果指定 `false`，需为每个使用的策略添加重定向 URI。 例如：`https://{your-tenant-name}.b2clogin.cn/{your-tenant-name}.partner.onmschina.cn/{policy-name}/oauth2/authresp`。 |
| MarkAsFailureOnStatusCode5xx | 否 | 指示在 Http 状态代码处于 5xx 范围内的情况下，是否应将对外部服务的请求标记为失败。 默认为 `false`。 |
| DiscoverMetadataByTokenIssuer | 否 | 指示是否应使用 JWT 令牌中的颁发者来发现 OIDC 元数据。 |
| IncludeClaimResolvingInClaimsHandling  | 否 | 对于输入和输出声明，指定[声明解析](claim-resolver-overview.md)是否包含在技术配置文件中。 可能的值：`true` 或 `false` （默认值）。 若要使用技术配置文件中的声明解析程序，请将此项设为 `true`。 |

## <a name="cryptographic-keys"></a>加密密钥

**CryptographicKeys** 元素包含以下属性：

| 属性 | 必须 | 说明 |
| --------- | -------- | ----------- |
| client_secret | 是 | 标识提供者应用程序的客户端机密。 只有在将 **response_types** 元数据设置为 `code` 的情况下，才需要加密密钥。 在这种情况下，Azure AD B2C 会再次进行调用，以便用授权代码来交换访问令牌。 如果元数据已设置为 `id_token`，则可省略加密密钥。  |

## <a name="redirect-uri"></a>重定向 URI

配置标识提供者的重定向 URI 时，请输入 `https://{your-tenant-name}.b2clogin.cn/{your-tenant-name}.partner.onmschina.cn/oauth2/authresp`。 确保将 `{your-tenant-name}` 替换为你的租户名称。 重定向 URI 需要采用全小写形式。

<!-- Update_Description: wording update -->

