---
title: 在自定义策略中定义电话因素技术配置文件
titleSuffix: Azure AD B2C
description: 在 Azure Active Directory B2C 的自定义策略中定义电话因素技术配置文件
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/01/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: ebab6d3bc9d8e65915d254395d67d5c1cde3c647
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80581852"
---
# <a name="define-a-phone-factor-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>在 Azure Active Directory B2C 自定义策略中定义电话因素技术配置文件

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) 支持注册和验证电话号码。 此技术配置文件：

- 提供一个用来与用户交互的用户界面。
- 使用内容定义来控制外观。
- 支持使用电话呼叫和短信来验证电话号码。
- 支持多个电话号码。 用户可以选择一个要验证的电话号码。  
- 如果提供了一个电话号码，电话因素用户界面将要求用户验证该电话号码。 如果未提供电话号码，则它会要求用户注册新的电话号码。
- 返回一个声明，指示用户是否提供了新电话号码。 可以使用此声明来确定是否要将电话号码保存在 Azure AD 用户配置文件中。  

## <a name="protocol"></a>协议

“Protocol”元素的“Name”属性必须设置为 `Proprietary`。 handler 属性必须包含 Azure AD B2C 对电话因素使用的协议处理程序程序集的完全限定名称：`Web.TPEngine.Providers.PhoneFactorProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

以下示例演示了用于注册和验证的电话因素技术配置文件：

```XML
<TechnicalProfile Id="PhoneFactor-InputOrVerify">
  <DisplayName>PhoneFactor</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.PhoneFactorProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="input-claims"></a>输入声明

InputClaims 元素必须包含以下声明。 还可将声明名称映射到电话因素技术配置文件中定义的名称。 

```XML
<InputClaims>
  <!--A unique identifier of the user. The partner claim type must be set to `UserId`. -->
  <InputClaim ClaimTypeReferenceId="userIdForMFA" PartnerClaimType="UserId" />
  <!--A claim that contains the phone number. If the claim is empty, Azure AD B2C asks the user to enroll a new phone number. Otherwise, it asks the user to verify the phone number. -->
  <InputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />
</InputClaims>
```

以下示例演示如何使用多个电话号码。 有关详细信息，请参阅[示例策略](https://github.com/azure-ad-b2c/samples/tree/master/policies/mfa-add-secondarymfa)。

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="userIdForMFA" PartnerClaimType="UserId" />
  <InputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />
  <InputClaim ClaimTypeReferenceId="secondaryStrongAuthenticationPhoneNumber" />
</InputClaims>
```

InputClaimsTransformations 元素可以包含用于修改输入声明，或者生成新的声明，然后在电话因素页中显示这些声明的 InputClaimsTransformation 元素集合。

## <a name="output-claims"></a>输出声明

OutputClaims 元素包含电话因素技术配置文件返回的声明列表。

```xml
<OutputClaims>
  <!-- The verified phone number. The partner claim type must be set to `Verified.OfficePhone`. -->
  <OutputClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" PartnerClaimType="Verified.OfficePhone" />
  <!-- Indicates whether the new phone number has been entered by the user. The partner claim type must be set to `newPhoneNumberEntered`. -->
  <OutputClaim ClaimTypeReferenceId="newPhoneNumberEntered" PartnerClaimType="newPhoneNumberEntered" />
</OutputClaims>
```

OutputClaimsTransformations 元素可以包含用于修改输出声明或生成新输出声明的 OutputClaimsTransformation 元素集合。

## <a name="cryptographic-keys"></a>加密密钥

不使用“CryptographicKeys”  元素。


## <a name="metadata"></a>元数据

| Attribute | 必选 | 说明 |
| --------- | -------- | ----------- |
| ContentDefinitionReferenceId | 是 | 与此技术配置文件关联的[内容定义](contentdefinitions.md)的标识符。 |
| ManualPhoneNumberEntryAllowed| 否 | 指定是否允许用户手动输入电话号码。 可能的值：`true` 或 `false`（默认值）。|

### <a name="ui-elements"></a>UI 元素

可以[本地化](localization-string-ids.md#azure-mfa-error-messages)电话因素身份验证页的用户界面元素。

## <a name="next-steps"></a>后续步骤

- 查看[使用 MFA 的社交和本地帐户](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/SocialAndLocalAccountsWithMfa)初学者包。


