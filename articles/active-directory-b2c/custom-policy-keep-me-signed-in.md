---
title: 在 Azure Active Directory B2C 中使我保持登录状态
description: 了解如何在 Azure Active Directory B2C 中设置“使我保持登录状态 (KMSI)”。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 234433267b5f83e7be6d5dd6120c03b205f82f28
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78265970"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中启用“使我保持登录状态 (KMSI)”

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

你可以为在 Azure Active Directory B2C (Azure AD B2C) 目录中拥有本地帐户的 Web 和本机应用程序的用户启用“使我保持登录状态 (KMSI)”功能。 此功能会向返回到应用程序的用户授予访问权限，而不会提示他们重新输入用户名和密码。 当用户注销时，会撤销此访问权限。

用户不应在公用计算机上启用此选项。

![显示“使我保持登录状态”复选框的示例注册登录页](./media/custom-policy-keep-me-signed-in/kmsi.PNG)

## <a name="prerequisites"></a>必备条件

- 配置为允许本地帐户登录的 Azure AD B2C 租户。 外部标识提供者帐户不支持 KMSI。

## <a name="configure-the-page-identifier"></a>配置页面标识符

若要启用 KMSI，请将内容定义 `DataUri` 元素设置为[页面标识符](contentdefinitions.md#datauri) `unifiedssp` 并将[页面版本](page-layout.md)设置为 *1.1.0* 或更高版本。

1. 打开策略的扩展文件。 例如<em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>。 此扩展文件是自定义策略初学者包中包含的策略文件之一，你在先决条件“自定义策略入门”中应该已获取了该包。
1. 搜索 BuildingBlocks  元素。 如果该元素不存在，请添加该元素。
1. 将 **ContentDefinitions** 元素添加到策略的 **BuildingBlocks** 元素。

    你的自定义策略应当如以下代码片段所示：

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

1. 保存扩展文件。



## <a name="configure-a-relying-party-file"></a>配置信赖方文件

更新用于启动创建的用户旅程的信赖方 (RP) 文件。

1. 打开自定义策略文件。 例如，SignUpOrSignin.xml  。
1. 如果它尚不存在，请将 `<UserJourneyBehaviors>` 子节点添加到 `<RelyingParty>` 节点。 它必须紧跟在 `<DefaultUserJourney ReferenceId="User journey Id" />` 之后，例如：`<DefaultUserJourney ReferenceId="SignUpOrSignIn" />`。
1. 将以下节点添加为 `<UserJourneyBehaviors>` 元素的子级。

    ```XML
    <UserJourneyBehaviors>
      <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
      <SessionExpiryType>Absolute</SessionExpiryType>
      <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
    </UserJourneyBehaviors>
    ```

    - **SessionExpiryType** - 指示如何按 `SessionExpiryInSeconds` 和 `KeepAliveInDays` 中指定的时间延长会话。 `Rolling` 值（默认值）指示每当用户执行身份验证时都延长会话。 `Absolute` 值指示在指定的时间段后将强制用户重新进行身份验证。

    - **SessionExpiryInSeconds** - 未启用“使我保持登录状态”  或者用户未选择“使我保持登录状态”  时，会话 cookie 的生存期。 会话在经过 `SessionExpiryInSeconds` 后或浏览器关闭时到期。

    - **KeepAliveInDays** - 当启用了“使我保持登录状态”  并且用户选择了“使我保持登录状态”  时，会话 cookie 的生存期。  `KeepAliveInDays` 的值优先于 `SessionExpiryInSeconds` 值，并指示会话到期时间。 如果用户关闭浏览器后再重新打开，只要还在 KeepAliveInDays 时间段内，用户仍然可以静默登录。

    有关详细信息，请参阅[用户旅程行为](relyingparty.md#userjourneybehaviors)。

建议将 SessionExpiryInSeconds 的值设置为较短时间段（1200 秒），而将 KeepAliveInDays 的值设置为较长时间段（30 天），如下例所示：

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <UserJourneyBehaviors>
    <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
    <SessionExpiryType>Absolute</SessionExpiryType>
    <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
  </UserJourneyBehaviors>
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
      <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

4. 保存更改，然后上传文件。
5. 若要测试所上传的自定义策略，请在 Azure 门户中转到策略页，并选择“立即运行”  。

可在[此处](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in)找到示例策略。

