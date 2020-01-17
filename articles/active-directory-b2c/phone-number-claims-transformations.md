---
title: 自定义策略中的电话号码声明转换
titleSuffix: Azure AD B2C
description: Azure AD B2C 中电话号码声明转换的自定义策略参考。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 2652182dab0da7842213a5cc5c7abed3118bce46
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624392"
---
# <a name="define-phone-number-claims-transformations-in-azure-ad-b2c"></a>定义 Azure AD B2C 中的电话号码声明转换

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

本文提供了有关在 Azure Active Directory B2C (Azure AD B2C) 中使用 Identity Experience Framework 架构的电话号码声明转换的参考和示例。 有关一般声明转换的详细信息，请参阅 [ClaimsTransformations](claimstransformations.md)。

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="convertstringtophonenumberclaim"></a>ConvertStringToPhoneNumberClaim

此声明验证电话号码的格式。 如果它的格式有效，则将其更改为 Azure AD B2C 使用的标准格式。 如果提供的电话号码的格式无效，则返回一条错误消息。

| 项目 | TransformationClaimType | 数据类型 | 注释 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | string | 要进行转换的字符串类型的声明。 |
| OutputClaim | outputClaim | string | 此声明转换的结果。 |

ConvertStringToPhoneNumberClaim 声明转换始终通过[验证技术配置文件](validation-technical-profile.md)执行，该文件由[自断言技术配置文件](self-asserted-technical-profile.md)或[显示控制](display-controls.md)调用  。 UserMessageIfClaimsTransformationInvalidPhoneNumber 自断言技术配置文件元数据控制向用户显示的错误消息  。

![错误消息执行路径示意图](./media/phone-authentication/assert-execution.png)

使用此声明转换可确保所提供的字符串声明是有效的电话号码。 如果没有，则会引发错误消息。 下面的示例检查 phoneString ClaimType 是否确实是有效的电话号码，然后以标准的 Azure AD B2C 格式返回该电话号码  。 否则会引发错误消息。

```XML
<ClaimsTransformation Id="ConvertStringToPhoneNumber" TransformationMethod="ConvertStringToPhoneNumberClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="phoneString" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="phoneNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

如果自断言技术配置文件调用包含此声明转换的验证技术配置文件，则可定义错误消息。

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignup-Phone">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationInvalidPhoneNumber">Custom error message if the phone number is not valid.</Item>
  </Metadata>
  ...
</TechnicalProfile>
```

### <a name="example"></a>示例

- 输入声明：
  - inputClaim：+1 (123) 456-7890 
- 输出声明：
  - outputClaim：+11234567890 

## <a name="getnationalnumberandcountrycodefromphonenumberstring"></a>GetNationalNumberAndCountryCodeFromPhoneNumberString

这将从输入声明中提取国家/地区代码和国家/地区号码，如果提供的电话号码无效，则可选择引发异常。

| 项目 | TransformationClaimType | 数据类型 | 注释 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | phoneNumber | string | 电话号码的字符串声明。 电话号码必须采用国际格式，带有前导“+”和国家/地区代码。 |
| InputParameter | throwExceptionOnFailure | boolean | [可选]一个参数，指示当电话号码无效时是否引发异常。 默认值为 false。 |
| InputParameter | countryCodeType | string | [可选]一个参数，指示输出声明中国家/地区代码的类型。 可用值为 CallingCode（国家/地区的国际呼叫代码，例如 +1）或 ISO3166（两个字母的 ISO-3166 国家/地区代码）   。 |
| OutputClaim | nationalNumber | string | 电话号码的国家/地区号码的字符串声明。 |
| OutputClaim | countryCode | string | 电话号码的国家/地区代码的字符串声明。 |


如果 GetNationalNumberAndCountryCodeFromPhoneNumberString 声明转换通过[验证技术配置文件](validation-technical-profile.md)执行，该文件由[自断言技术配置文件](self-asserted-technical-profile.md)或[显示控制操作](display-controls.md#display-control-actions)调用，则 UserMessageIfPhoneNumberParseFailure 自断言技术配置文件元数据可控制向用户显示的错误消息   。

![错误消息执行路径示意图](./media/phone-authentication/assert-execution.png)

此声明转换可用于将完整的电话号码拆分为国家/地区代码和国家/地区号码。 如果提供的电话号码无效，则可选择引发错误消息。

下面的示例尝试将电话号码拆分为国家/地区号码和国家/地区代码。 如果电话号码有效，则电话号码将由国家/地区号码替代。 如果电话号码无效，将不会引发异常，并且电话号码将仍为其原始值。

```XML
<ClaimsTransformation Id="GetNationalNumberAndCountryCodeFromPhoneNumberString" TransformationMethod="GetNationalNumberAndCountryCodeFromPhoneNumberString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="phoneNumber" TransformationClaimType="phoneNumber" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="throwExceptionOnFailure" DataType="boolean" Value="false" />
    <InputParameter Id="countryCodeType" DataType="string" Value="ISO3166" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="nationalNumber" TransformationClaimType="nationalNumber" />
    <OutputClaim ClaimTypeReferenceId="countryCode" TransformationClaimType="countryCode" />
  </OutputClaims>
</ClaimsTransformation>
```

如果自断言技术配置文件调用包含此声明转换的验证技术配置文件，则可定义错误消息。

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignup-Phone">
  <Metadata>
    <Item Key="UserMessageIfPhoneNumberParseFailure">Custom error message if the phone number is not valid.</Item>
  </Metadata>
  ...
</TechnicalProfile>
```

### <a name="example-1"></a>示例 1

- 输入声明：
  - phoneNumber：+49 (123) 456-7890 
- 输入参数：
  - throwExceptionOnFailure：false 
  - countryCodeType  ：ISO3166
- 输出声明：
  - nationalNumber  ：1234567890
  - countryCode  ：DE

### <a name="example-2"></a>示例 2

- 输入声明：
  - phoneNumber：+49 (123) 456-7890 
- 输入参数
  - throwExceptionOnFailure：false 
  - countryCodeType  ：CallingCode
- 输出声明：
  - nationalNumber  ：1234567890
  - countryCode：+49 

