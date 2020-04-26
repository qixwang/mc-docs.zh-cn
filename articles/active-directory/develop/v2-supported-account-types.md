---
title: 支持的帐户类型 - Microsoft 标识平台 | Azure
description: 有关受众的概念文档以及应用程序中受支持的帐户类型
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/23/2020
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: bd46e1d576c74550494e35a162610e5567f80283
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126381"
---
# <a name="supported-account-types"></a>支持的帐户类型

本文介绍了应用程序支持的帐户类型（有时称为受众）。

<!-- This section can be in an include for many of the scenarios (SPA, web app signing-in users, protecting a web API, Desktop (depending on the flows), Mobile -->

## <a name="supported-accounts-types-in-microsoft-identity-platform-applications"></a>Microsoft 标识平台应用程序中支持的帐户类型

大多数类型的应用都可以使用任何受众登录用户：

- 如果你正在编写业务线 (LOB) 应用程序，则可以在自己的组织中登录用户。 此类应用程序有时被命名为“单租户”  。
- 如果你为 ISV，则可以编写一个应用程序来登录用户：

  - 在任何组织中。 这样的应用程序被命名为“多租户”Web 应用程序  。 你有时会看到它使用工作或学校帐户登录用户。
  - 通过其工作或学校。

- 如果你正在为消费者应用程序编写业务，还可以借助 Azure AD B2C 使用其社交身份登录用户。

## <a name="certain-authentication-flows-dont-support-all-the-account-types"></a>某些身份验证流程并不支持所有帐户类型

某些帐户类型不能与某些身份验证流程一起使用。 例如，在桌面、UWP 应用程序或守护程序应用程序中：

- 守护程序应用程序只能与 Azure Active Directory 组织配合使用。 
- 只能将集成身份验证流与工作或学校帐户配合使用（在你的组织或任何组织中均可）。 实际上，集成 Windows 身份验证适用于域帐户，并且需要将计算机加入域或加入 Azure AD。 
- [资源所有者密码授予](./v2-oauth-ropc.md)（用户名/密码）

## <a name="supported-account-types-in-national-clouds"></a>国家/地区云支持的帐户类型

 应用还可以在[国家/地区云](authentication-national-cloud.md)中登录用户。 正因如此，这些云才将支持的帐户类型减少到你的组织（单个租户）或任何组织（多租户应用程序）。

## <a name="next-steps"></a>后续步骤

- 了解有关 [Azure Active Directory 中的租赁](./single-and-multi-tenant-apps.md)的详细信息
- 了解有关[国家/地区云](./authentication-national-cloud.md)的详细信息

