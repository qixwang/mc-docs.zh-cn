---
title: 添加或删除用户 - Azure Active Directory | Microsoft Docs
description: 有关如何使用 Azure Active Directory 添加新用户或删除现有用户的说明。
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
origin.date: 11/12/2019
ms.date: 11/28/2019
ms.author: v-junlch
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 545834f4c893fa5e0cd9bbe67790b40183556fab
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82507634"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>使用 Azure Active Directory 添加或删除用户

在 Azure Active Directory (Azure AD) 组织中添加新用户或删除现有用户。 只有用户管理员或全局管理员可以添加或删除用户。

## <a name="add-a-new-user"></a>添加新用户

可使用 Azure Active Directory 门户创建新用户。

若要添加新用户，请执行以下步骤：

1. 以组织的用户管理员身份登录到 [Azure 门户](https://portal.azure.cn/)。

1. 在任意页面中搜索并选择“Azure Active Directory”。 

1. 依次选择“用户”、“新建用户”。  

    ![通过“用户”（Azure AD 中的“所有用户”）添加用户](./media/add-users-azure-active-directory/add-user-in-users-all-users.png)

1. 在“用户”页上输入此用户的信息： 

   - **Name**。 必需。 新用户的名字和姓氏。 例如，*Mary Parker*。

   - **用户名**。 必需。 新用户的用户名。 例如，`mary@contoso.com`。

     用户名的域名部分必须是初始默认域名 *\<yourdomainname>.partner.onmschina.cn*，或者是一个自定义域名，例如 *contoso.com*。 若要详细了解如何创建自定义域名，请参阅[使用 Azure Active Directory 门户添加自定义域名](add-custom-domain.md)。

   - **组**。 （可选）可以将用户添加到一个或多个现有组。 也可以在以后将用户添加到组中。 有关将用户添加到组的详细信息，请参阅[使用 Azure Active Directory 创建基本组并添加成员](active-directory-groups-create-azure-portal.md)。

   - **目录角色**：如果需要用户的 Azure AD 管理权限，则可以将其添加到 Azure AD 角色。 可以将用户分配为全局管理员，或者分配为 Azure AD 中有限的管理员角色中的一个或多个。 有关分配角色的详细信息，请参阅[如何向用户分配角色](active-directory-users-assign-role-azure-portal.md)。

   - **工作信息**：可在此处添加有关用户的详细信息，或者以后再添加。 有关添加用户信息的详细信息，请参阅[如何添加或更改用户个人资料信息](active-directory-users-profile-azure-portal.md)。

1. 复制“密码”框中提供的自动生成的密码。  需将此密码提供给用户，供其在首次登录时使用。

1. 选择“创建”  。

随即会创建该用户，并将其添加到 Azure AD 组织中。

## <a name="add-a-consumer-user"></a>添加使用者用户

某些情况下，可能需要在 Azure Active Directory B2C (Azure AD B2C) 目录中手动创建所有者帐户。 

## <a name="add-a-new-user-within-a-hybrid-environment"></a>在混合环境内添加新用户

如果你的环境中同时包含 Azure Active Directory（云）和 Windows Server Active Directory（本地），则可以通过同步现有用户帐户数据来添加新用户。 有关混合环境和用户的详细信息，请参阅[将本地目录与 Azure Active Directory 集成](../hybrid/whatis-hybrid-identity.md)。

## <a name="delete-a-user"></a>删除用户

可使用 Azure Active Directory 门户删除现有用户。

若要删除用户，请执行以下步骤：

1. 使用组织的用户管理员帐户登录到 [Azure 门户](https://portal.azure.cn/)。

1. 在任意页面中搜索并选择“Azure Active Directory”。 

1. 搜索并选择要从 Azure AD 租户中删除的用户。 例如，_Mary Parker_。

1. 选择“删除用户”  。

    ![“用户 - 所有用户”页，其中突出显示了“删除用户”](./media/add-users-azure-active-directory/delete-user-all-users-blade.png)

用户将被删除并且不再显示在“用户 - 所有用户”页上。  可在接下来的 30 天内于“已删除用户”页查看该用户，在此期间可将其还原  。 有关还原用户的详细信息，请参阅[使用 Azure Active Directory 还原或永久删除最近删除的用户](active-directory-users-restore.md)。

删除某个用户后，该用户使用的任何许可证可供其他用户使用。

>[!Note]
>必须使用 Windows Server Active Directory 更新其授权来源为 Windows Server Active Directory 的用户的标识、联系信息或工作信息。 完成更新后，必须等待下一个同步循环完成，然后才能看到所做的更改。

## <a name="next-steps"></a>后续步骤

添加用户后，可以执行以下基本流程：

- [添加或更改个人资料信息](active-directory-users-profile-azure-portal.md)

- [向用户分配角色](active-directory-users-assign-role-azure-portal.md)

- [创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

或者，可以执行其他用户管理任务，例如[还原已删除的用户](active-directory-users-restore.md)。 有关其他可用操作的详细信息，请参阅 [Azure Active Directory 用户管理和文档](../users-groups-roles/index.yml)。

<!-- Update_Description: wording update -->