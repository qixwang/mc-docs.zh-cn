---
title: 使用组管理应用和资源访问 - Azure AD
description: 了解如何使用 Azure Active Directory 组来管理对组织的基于云的应用、本地应用和资源的访问。
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: v-junlch
ms.reviewer: piotrci
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b09d7e75cc5fc3bcaf97cfb690c4f63cbbf78ff
ms.sourcegitcommit: 48d51745ca18de7fa05b77501b4a9bf16cea2068
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76116771"
---
# <a name="manage-app-and-resource-access-using-azure-active-directory-groups"></a>使用 Azure Active Directory 组管理应用和资源访问
通过 Azure Active Directory (Azure AD) 可以使用组来管理基于云的应用、本地应用和资源。 资源可以是 Azure AD 组织的一部分（例如用于通过 Azure AD 中的角色管理对象的权限），也可以是组织外部的资源（Azure 服务、SharePoint 站点和本地资源）。

>[!NOTE]
>要使用 Azure Active Directory，需要一个 Azure 帐户。 如果没有帐户，可以[注册 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial/)。
>
> 在 Azure 门户中，可以看到一些组，其成员身份和组详细信息无法在门户中进行管理：
>
> - 从本地 Active Directory 同步的组只能在本地 Active Directory 中进行管理。
> - 其他组类型（如通讯组列表和启用了邮件的安全组）仅在 Exchange 管理中心或 Microsoft 365 管理中心进行管理。 必须登录到 Exchange 管理中心或 Microsoft 365 管理中心才能管理这些组。

## <a name="how-access-management-in-azure-ad-works"></a>Azure AD 中的访问管理如何工作

Azure AD 通过向单个用户或整个 Azure AD 组提供访问权限，帮助你授予组织资源的访问权限。 资源所有者（或 Azure AD 目录所有者）可以使用组将一组访问权限分配给组的所有成员，而无需逐个地提供权限。 资源或目录所有者还可将成员列表的管理权限授予其他某人（例如部门经理或支持管理员），让此人根据需要添加和删除成员。 有关如何管理组所有者的详细信息，请参阅[管理组所有者](active-directory-accessmanagement-managing-group-owners.md)

![Azure Active Directory 访问管理示意图](./media/active-directory-manage-groups/active-directory-access-management-works.png)

## <a name="ways-to-assign-access-rights"></a>分配访问权限的方式

可通过四种方式将资源访问权限分配给用户：

- **直接分配。** 资源所有者直接将用户分配到资源。

- **组分配。** 资源所有者将 Azure AD 组分配到资源，这会自动向所有组成员授予对该资源的访问权限。 组成员身份由组所有者和资源所有者管理，允许任一所有者在该组中添加或删除成员。 有关添加或删除组成员的详细信息，请参阅[如何：使用 Azure Active Directory 门户在一个组中添加或删除另一个组](active-directory-groups-membership-azure-portal.md)。 

- **External authority assignment**（外部机构分配）。 访问来自外部源，例如本地目录。 在这种情况下，资源所有者将分配一个组以提供资源访问权限，外部源将管理组成员。

   ![访问管理示意图概览](./media/active-directory-manage-groups/access-management-overview.png)

## <a name="next-steps"></a>后续步骤
大致了解如何使用组进行访问管理之后，可以开始管理资源和应用。

- [使用 Azure Active Directory 创建新组](active-directory-groups-create-azure-portal.md)或[使用 PowerShell cmdlet 创建和管理新组](../users-groups-roles/groups-settings-v2-cmdlets.md)

- [使用 Azure AD Connect 将本地组同步到 Azure](../connect/active-directory-aadconnect.md)

- [使用 Azure AD Connect 将本地组同步到 Azure](../hybrid/whatis-hybrid-identity.md)
<!-- Update_Description: wording update -->
