---
title: 新的 Azure 门户应用注册培训指南
description: 介绍新的 Azure 门户应用注册体验
services: active-directory
author: archieag
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: v-junlch
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12a5768275ed531c756963930a0266c277fcd80a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75776856"
---
# <a name="new-azure-portal-app-registration-training-guide"></a>新的 Azure 门户应用注册培训指南

在 Azure 门户上的新[应用注册](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)体验中，可以发现许多的改进。 如果你熟悉 Azure 门户中的应用注册（旧版）体验，可以参考本培训指南开始使用新的体验。

在 Azure Active Directory 中，本文所述的新应用程序注册体验已推出正式版 (GA)。 在 Azure Active Directory B2C (Azure AD B2C) 中，此体验以预览版提供。

## <a name="key-changes"></a>关键更改

- 应用注册不局限于 *Web 应用/API* 或*本机*应用。 注册相应的重定向 URI，即可对所有这些应用使用同一个应用注册。

- 旧体验仅支持使用组织 (Azure AD) 帐户登录的应用。 应用注册为单租户。 应用仅支持应用已注册到的目录中的组织帐户。 可将应用修改为多租户，然后它们可以支持所有组织帐户。 在新体验中，可以注册支持这两个选项加上第三个选项的应用：所有组织帐户。

- 仅当已使用组织帐户登录到 Azure 门户后，才可以使用旧体验。 

## <a name="list-of-applications"></a>应用程序列表

新应用列表显示通过 Azure 门户中旧应用注册体验注册的应用程序。 这些应用使用 Azure AD 帐户登录。

新应用列表不包含“应用程序类型”列，因为单个应用注册可能属于多种类型。  此列表包含两个附加的列：“创建时间”及“证书和机密”。   “证书和机密”显示已在应用中注册的凭据的状态。  状态包括“当前”、“即将过期”和“已过期”。   

## <a name="new-app-registration"></a>新应用注册

在旧体验中，若要注册应用，需要提供：“名称”、“应用程序类型”和“单一登录 URL/重定向 URI”。    创建的应用是仅限 Azure AD 的单租户应用程序。 它们仅支持应用已注册到的目录中的组织帐户。

在新体验中，必须提供应用的**名称**，并选择**支持的帐户类型**。 可以选择性地提供**重定向 URI**。 如果提供重定向 URI，需要指定它是否为 Web/公共 URI（移动和桌面）。 有关详细信息，请参阅[快速入门：将应用程序注册到 Microsoft 标识平台](quickstart-register-app.md)。 对于 Azure AD B2C，请参阅[在 Azure Active Directory B2C 中注册应用程序](../../active-directory-b2c/tutorial-register-applications.md)。


## <a name="deleting-an-app-registration"></a>删除应用注册

在旧体验中，只能删除单租户应用。 对于多租户应用，已禁用删除按钮。 在新体验中，可以删除处于任何状态的应用，但必须确认该操作。 有关详细信息，请参阅[快速入门：删除注册到 Microsoft 标识平台的应用程序](quickstart-remove-app.md)。

## <a name="application-manifest"></a>应用程序清单

旧体验和新体验对清单编辑器中的 JSON 格式使用不同的版本。 有关详细信息，请参阅 [Azure Active Directory 应用清单](reference-app-manifest.md)。

## <a name="new-ui"></a>新 UI

新体验为以下属性添加了 UI 控件：

- “身份验证”页包含“隐式授权流”(`oauth2AllowImplicitFlow`)。   与在旧体验中不同，现在可以启用“访问令牌”和/或“ID 令牌”。  
- “公开 API”页包含“此 API 定义的范围”(`oauth2Permissions`) 和“已授权的客户端应用程序”(`preAuthorizedApplications`)。    有关如何将应用配置为 Web API 和公开权限/范围的详细信息，请参阅[快速入门：配置应用程序以公开 Web API](quickstart-configure-app-expose-web-apis.md)。
- “品牌”页包含“发布者域”。   发布者域将在[应用程序的许可提示](application-consent-experience.md)中向用户显示。 有关更多信息，请参阅[如何：配置应用程序的发布者域](howto-configure-publisher-domain.md)。

## <a name="limitations"></a>限制

新体验具有以下限制：

- 客户端机密（应用密码）的格式不同于旧体验中的格式，可能会中断 CLI。
- 不支持在 UI 中更改受支持帐户的值。 除非在 Azure AD 单租户与多租户之间切换，否则需要使用应用清单。

<!-- Update_Description: wording update -->