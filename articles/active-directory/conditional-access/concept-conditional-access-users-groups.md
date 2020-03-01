---
title: 条件访问策略中的用户和组 - Azure Active Directory
description: Azure AD 条件访问策略中的用户和组包括谁
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: a04f3609fedbf925c2dfea92f5209a46252dceac
ms.sourcegitcommit: afe972418a883551e36ede8deae32ba6528fb8dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77541115"
---
# <a name="conditional-access-users-and-groups"></a>条件访问：用户和组

条件访问策略必须包括用户分配，作为决策过程中的信号之一。 可以在条件访问策略中包括或排除用户。 

![用户作为条件访问所做决策中的信号](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups.png)

## <a name="include-users"></a>包括用户

此用户列表通常包括组织在条件访问策略中设为目标的所有用户。 

创建条件访问策略时，可以包括以下选项。

- 无
   - 不选择任何用户
- 所有用户
   - 目录中存在的所有用户。
- 选择“用户和组”
   - 所有来宾和外部用户(预览版)
      - 此选择包括所有外部用户，包括将 `user type` 特性设置为 `guest` 的任何用户。 此选择也适用于从其他组织（例如云解决方案提供商 (CSP)）登录的任何外部用户。 
   - 目录角色(预览版)
      - 允许管理员选择用于确定分配的特定 Azure AD 目录角色。 例如，组织可以对分配有全局管理员角色的用户创建更严格的策略。
   - 用户和组
      - 允许以特定用户集为目标。 例如，将某个人力资源应用选作云应用时，组织可以选择包含人力资源部所有成员的组。 某个组可以是 Azure AD 中任何类型的组，包括动态组，或分配的安全组和通讯组。

## <a name="exclude-users"></a>排除用户

排除通常用于紧急访问或不受限帐户。 有关紧急访问帐户及其重要原因的详细信息，请参阅以下文章： 

* [在 Azure AD 中管理紧急访问帐户](../users-groups-roles/directory-emergency-access.md)

创建条件访问策略时，可以排除以下选项。

- 所有来宾和外部用户(预览版)
   - 此选择包括所有外部用户，包括将 `user type` 特性设置为 `guest` 的任何用户。 此选择也适用于从其他组织（例如云解决方案提供商 (CSP)）登录的任何外部用户。 
- 目录角色(预览版)
   - 允许管理员选择用于确定分配的特定 Azure AD 目录角色。 例如，组织可以对分配有全局管理员角色的用户创建更严格的策略。
- 用户和组
   - 允许以特定用户集为目标。 例如，将某个人力资源应用选作云应用时，组织可以选择包含人力资源部所有成员的组。 某个组可以是 Azure AD 中任何类型的组，包括动态组，或分配的安全组和通讯组。

## <a name="next-steps"></a>后续步骤

- [条件访问：云应用或操作](concept-conditional-access-cloud-apps.md)

- [条件访问常见策略](concept-conditional-access-policy-common.md)

