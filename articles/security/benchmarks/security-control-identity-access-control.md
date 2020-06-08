---
title: Azure 安全控制 - 标识和访问控制
description: Azure 安全控制标识和访问控制
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: v-tawe
ms.custom: security-benchmark
origin.date: 04/14/2020
ms.openlocfilehash: 3af44dc291c53e18feba544e2ba497be1ebed2f7
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200089"
---
# <a name="security-control-identity-and-access-control"></a>安全控制：标识和访问控制

标识和访问管理建议侧重于解决与以下方面相关的问题：基于标识的访问控制、锁定管理访问权限、对与标识相关的事件发出警报、异常帐户行为和基于角色的访问控制。

## <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1：维护管理帐户的清单

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 3.1 | 4.1 | 客户 |

Azure AD 具有必须显式分配且可查询的内置角色。 使用 Azure AD PowerShell 模块执行即席查询，以发现属于管理组的成员的帐户。

- [如何使用 PowerShell 获取 Azure AD 中的目录角色](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [如何使用 PowerShell 获取 Azure AD 中目录角色的成员](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

## <a name="32-change-default-passwords-where-applicable"></a>3.2：在适用的情况下更改默认密码

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 3.2 | 4.2 | 客户 |

Azure AD 没有默认密码。 其他需要密码的 Azure 资源会强制创建具有复杂性要求和最小密码长度的密码，该长度因服务而异。 你对可能使用默认密码的第三方应用程序和市场服务负责。

## <a name="33-use-dedicated-administrative-accounts"></a>3.3：使用专用管理帐户

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 3.3 | 4.3 | 客户 |

围绕专用管理帐户的使用创建标准操作程序。 使用 Azure 安全中心标识和访问管理来监视管理帐户的数量。

还可以通过使用 Microsoft 服务的 Azure AD Privileged Identity Management 特权角色和 Azure 资源管理器来启用实时/足够访问权限。 

- [详细了解 Privileged Identity Management](https://docs.azure.cn/active-directory/privileged-identity-management/)



## <a name="34-use-azure-active-directory"></a>3.4：使用 Azure Active Directory

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 3.9 | 16.1、16.2、16.4、16.5、16.6 | 客户 |

使用 Azure Active Directory (AAD) 作为中心身份验证和授权系统。 AAD 通过对静态数据和传输中数据使用强加密来保护数据。 AAD 还会对用户凭据进行加盐、哈希处理和安全存储。

- [如何创建和配置 AAD 实例](https://docs.azure.cn/active-directory/fundamentals/active-directory-access-create-new-tenant)

## <a name="35-regularly-review-and-reconcile-user-access"></a>3.5：定期审查和协调用户访问

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 3.10 | 16.9、16.10 | 客户 |

Azure AD 提供日志来帮助发现过时的帐户。 此外，请使用 Azure 标识访问评审来有效管理组成员身份、对企业应用程序的访问和角色分配。 可以定期评审用户的访问权限，确保只有适当的用户才持续拥有访问权限。 

- [了解 Azure AD 报告](https://docs.azure.cn/active-directory/reports-monitoring/)

- [如何使用 Azure 标识访问评审](https://docs.azure.cn/active-directory/governance/access-reviews-overview)



## <a name="next-steps"></a>后续步骤

- 请参阅下一个安全控制：[数据保护](security-control-data-protection.md)