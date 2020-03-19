---
title: 将 Active Directory 与 Azure Active Directory 进行比较
description: 本文档将 Active Directory 域服务 (ADDS) 与 Azure Active Directory (AD) 进行了比较。 它概述了这两个标识解决方案中的关键概念，并解释了它们的不同或相似之处。
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 03/10/2020
ms.author: v-junlch
ms.openlocfilehash: 6d819266827282dc1d0e15bc6440b9eb1e944ade
ms.sourcegitcommit: 4ba6d7c8bed5398f37eb37cf5e2acafcdcc28791
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79134178"
---
# <a name="compare-active-directory-to-azure-active-directory"></a>将 Active Directory 与 Azure Active Directory 进行比较

Azure Active Directory 是适用于云的标识和访问管理解决方案的下一次革命。 Microsoft 在 Windows 2000 中引入了 Active Directory 域服务，使组织能够使用每个用户的单一标识管理多个本地基础结构组件和系统。

Azure AD 将这一方法提升到了一个新的层次，它为各种组织提供了一个新的标识访问解决方案，同时适用于云中和本地的所有应用程序。

大多数 IT 管理员都熟悉 Active Directory 域服务概念。 下表概述了 Active Directory 概念与 Azure Active Directory 之间的差异和相似之处。

|概念|Active Directory (AD)|Azure Active Directory |
|:-|:-|:-|
|**用户**|||
| 权利管理和组| 管理员使用户成为组的成员。 然后，应用和资源所有者向组授予对应用或资源的访问权限。| Azure AD 中也提供了[组](/active-directory/fundamentals/active-directory-groups-create-azure-portal)，管理员也可以使用组来授予对资源的权限。 在 Azure AD 中，管理员可以手动将成员身份分配到组，也可以使用查询动态地将用户包括到组中。 |
| 管理员管理|组织将在 AD 中使用域、组织单位和组的组合来委派管理权限，以管理其控制的目录和资源。| Azure AD 通过其基于角色的访问控制 (RBAC) 系统提供了[内置角色](/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)，并提供了相应的功能来[创建自定义角色](/active-directory/users-groups-roles/roles-custom-overview)以委派对标识系统及其控制的应用和资源的特权访问。 </br>可以通过 [Privileged Identity Management (PIM)](/active-directory/privileged-identity-management/pim-configure) 增强对角色的管理，以提供对特权角色的实时、有时间限制或基于工作流的访问。 |
| 凭据管理| Active Directory 中的凭据基于密码、证书身份验证和智能卡身份验证。 密码是使用基于密码长度、有效期和复杂性的密码策略管理的。|Azure AD 对云和本地都使用智能密码保护。 保护包括智能锁定，以及阻止通用和自定义密码短语和替换。 </br>Azure AD [通过多重身份验证](/active-directory/authentication/concept-mfa-howitworks)技术（例如 FIDO2）显著提高了安全性。 </br>Azure AD 通过向用户提供[自助式密码重置](/active-directory/authentication/concept-sspr-howitworks)系统来降低支持成本。 |
| **应用**|||
| 基础结构应用|Active Directory 构成了许多基础结构本地组件的基础，例如 DNS、DHCP、IPSec、WiFi、NPS 和 VPN 访问|在新的云环境中，Azure AD 是用于访问应用的新控制平面，而不是依赖于网络控制。 当用户进行身份验证时，条件访问 (CA) 将控制在所需条件下哪些用户可以访问哪些应用程序。|
| 采用新式身份验证的业务线 (LOB) 应用|组织可以将 AD FS 与 Active Directory 配合使用来支持需要新式身份验证的 LOB 应用。| 需要新式身份验证的 LOB 应用可以配置为使用 Azure AD 进行身份验证。 |
| 中间层级/守护程序服务|在本地环境中运行的服务通常使用 AD 服务帐户来运行。 然后，这些应用将继承服务帐户的权限。| Azure AD 提供[托管标识](/active-directory/managed-identities-azure-resources/index)，以在云中运行其他工作负荷。 这些标识的生命周期由 Azure AD 管理，并绑定到资源提供程序，无法用于其他目的以获得后门访问。|
| **设备**|||
| 移动|在没有第三方解决方案的情况下，Active Directory 本身不支持移动设备。| Microsoft 的移动设备管理解决方案 Microsoft Intune 集成了 Azure AD。 Microsoft Intune 向标识系统提供在身份验证期间要评估的设备状态信息。 |
| Windows 服务器| Active Directory 为使用组策略或其他管理解决方案的本地 Windows 服务器提供了强大的管理功能。| 当 VM 需要访问标识系统目录或资源时，可以使用[托管标识](/active-directory/managed-identities-azure-resources/index)。|
| Linux/Unix 工作负荷|在没有第三方解决方案的情况下，Active Directory 本身不支持非 Windows 系统。|Linux/Unix VM 可以使用[托管标识](/active-directory/managed-identities-azure-resources/index)来访问标识系统或资源。 某些组织将这些工作负荷迁移到云容器技术，这些技术也可以使用托管标识。|

## <a name="next-steps"></a>后续步骤

- [什么是 Azure Active Directory？](/active-directory/fundamentals/active-directory-whatis)
- [有关 Azure Active Directory 的常见问题解答](/active-directory/fundamentals/active-directory-faq)

