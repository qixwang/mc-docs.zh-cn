---
title: 获得多个资源的同意（适用于 .NET 的 Microsoft 身份验证库）| Azure
description: 了解用户如何使用适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 获得多个资源的预先同意。
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 04/30/2019
ms.date: 06/18/2019
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7953eea850b7f042672cdfadd9dce1aba7cd83d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67305951"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>用户使用 MSAL.NET 获得多个资源的同意
Microsoft 标识平台终结点不允许你一次获取多个资源的令牌。 使用适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 时，获取令牌方法中的 scopes 参数应仅包含单个资源的范围。 但是，可以通过使用 `.WithExtraScopeToConsent` 生成器方法指定其他范围来预先同意多个资源。

> [!NOTE]
> 获得多个资源的同意适用于 Microsoft 标识平台，但不适用于 Azure AD B2C。 Azure AD B2C 仅支持管理员同意，不支持用户同意。

例如，如果你有两个资源（每个资源有两个范围）：

- https:\//mytenant.partner.onmschina.cn/customerapi（具有 2 个范围 `customer.read` 和 `customer.write`）
- https:\//mytenant.partner.onmschina.cn/vendorapi（具有 2 个范围 `vendor.read` 和 `vendor.write`）

应该使用具有 `.WithExtraScopeToConsent`extraScopesToConsent*参数的* 修饰符，如以下示例所示：

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.partner.onmschina.cn/customerapi/customer.read",
  "https://mytenant.partner.onmschina.cn/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.partner.onmschina.cn/vendorapi/vendor.read",
 "https://mytenant.partner.onmschina.cn/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

这将为你获得第一个 Web API 的访问令牌。 然后，当你需要访问第二个 Web API 时，可以从令牌缓存中以无提示方式获取令牌：

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

