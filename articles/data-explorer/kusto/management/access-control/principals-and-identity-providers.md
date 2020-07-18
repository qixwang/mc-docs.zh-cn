---
title: 主体和标识提供者 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的主体和标识提供者。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: 380dc705ad55ab8902792208df6062a2bfef4025
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226286"
---
# <a name="principals-and-identity-providers"></a>主体和标识提供者

Kusto 授权模型支持多个标识提供者 (IdP) 和多种主体类型。
本文回顾了支持的主体类型，并演示了如何将它们与[角色分配命令](../../management/security-roles.md)配合使用。

### <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory (AAD) 是 Azure 的首选多租户云目录服务和标识提供者，可以对安全主体单独进行身份验证，还可以与其他标识提供者（例如 Microsoft 的 Active Directory）联合进行身份验证。

AAD 是向 Kusto 进行身份验证的首选方法。 它支持多种身份验证方案：
* **用户身份验证**（交互式登录）：用于对人类主体进行身份验证。
* **应用程序身份验证**（非交互式登录）：用于对必须在没有人类用户参与的情况下运行或进行身份验证的服务和应用程序进行身份验证。

> [!NOTE]
> Azure Active Directory 不允许对服务帐户（根据定义，属于本地 AD 实体）进行身份验证。
AD 服务帐户的 AAD 等效项是 AAD 应用程序。

#### <a name="aad-group-principals"></a>AAD 组主体
Kusto 仅支持安全组主体（而不支持通讯组主体）。 尝试为 Kusto 群集上的 DG 设置访问权限会导致错误。

#### <a name="aad-tenants"></a>AAD 租户

如果未显式指定 AAD 租户，Kusto 会尝试从 UPN（UniversalPrincipalName，例如 `johndoe@fabrikam.com`）（如果已提供）来解析 AAD 租户。 如果你的主体不包含租户信息（未采用 UPN 格式），则必须通过将租户 ID 或名称追加到主体描述符来显式提及它。

AAD 主体的示例

|AAD 租户 |类型 |语法 |
|-----------|-----|-------|
|隐式 (UPN)  |User  |`aaduser=`UserEmailAddress
|显式 (ID)   |User  |`aaduser=`UserEmailAddress`;`TenantId 或 `aaduser=`ObjectID`;`TenantId
|显式（名称） |User  |`aaduser=`UserEmailAddress`;`TenantName 或 `aaduser=`ObjectID`;`TenantName
|隐式 (UPN)  |组 |`aadgroup=`GroupEmailAddress
|显式 (ID)   |组 |`aadgroup=`GroupObjectId`;`TenantId 或 `aadgroup=`GroupDisplayName`;`TenantId
|显式（名称） |组 |`aadgroup=`GroupObjectId`;`TenantName 或 `aadgroup=`GroupDisplayName`;`TenantName
|显式 (UPN)  |应用   |`aadapp`=ApplicationDisplayName`;`TenantId
|显式（名称） |应用   |`aadapp=`ApplicationId`;`TenantName

```kusto
// No need to specify AAD tenant for UPN, as Kusto performs the resolution by itself
.add database Test users ('aaduser=imikeoein@fabrikam.com') 'Test user (AAD)'

// AAD SG on 'fabrikam.com' tenant
.add database Test users ('aadgroup=SGDisplayName;fabrikam.com') 'Test group @fabrikam.com (AAD)'

// AAD App on 'fabrikam.com' tenant - by tenant name
.add database Test users ('aadapp=4c7e82bd-6adb-46c3-b413-fdd44834c69b;fabrikam.com') 'Test app @fabrikam.com (AAD)'
```

### <a name="microsoft-accounts-msas"></a>Microsoft 帐户 (MSA)
Microsoft 帐户 (MSA) 这个术语是指所有由 Microsoft 托管的非组织用户帐户，例如 `hotmail.com`、`live.com`、`outlook.com`。
Kusto 支持对 MSA 进行用户身份验证（注意，这里没有“安全组”这一概念），按 UPN（通用主体名称）验证身份。
在 Kusto 资源上配置 MSA 主体后，Kusto **不会**尝试解析提供的 UPN。

MSA 主体的示例

|IdP  |类型  |语法 |
|-----|------|-------|
|Live.com |User  |`msauser=`john.doe@live.com`

```kusto
.add database Test users ('msauser=john.doe@live.com') 'Test user (live.com)'
```

