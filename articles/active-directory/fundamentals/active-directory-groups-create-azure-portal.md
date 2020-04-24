---
title: 创建基本组并添成员 - Azure Active Directory | Microsoft Docs
description: 介绍如何使用 Azure Active Directory 创建基本组。
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: quickstart
origin.date: 03/01/2019
ms.date: 11/28/2019
ms.author: v-junlch
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 038d8b967bdf5c4a9851dc2ebb4a0b01983e346f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74655392"
---
# <a name="create-a-basic-group-and-add-members-using-azure-active-directory"></a>使用 Azure Active Directory 创建基本组并添成员
使用 Azure Active Directory (Azure AD) 门户可以创建基本组。 为了更好地阐述本文，资源所有者（管理员）将基本组添加到单个资源，基本组中包含了需要访问该资源的特定成员（员工）。 

## <a name="create-a-basic-group-and-add-members"></a>创建基本组并添加成员
创建基本组和添加成员可以同时进行。

### <a name="to-create-a-basic-group-and-add-members"></a>创建基本组并添加成员
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

1. 在“Active Directory”页面上，选择“组”，然后选择“新建组”    。

    ![显示组的 Azure AD 页](./media/active-directory-groups-create-azure-portal/group-full-screen.png)

1. 在“新建组”页面中，填写必填信息  。

    ![已填写示例信息的“新建组”页](./media/active-directory-groups-create-azure-portal/new-group-blade.png)

   - **组类型（必填）。** 选择预定义的组类型。 这包括：- **安全性**。 用于管理成员和一组用户对共享资源的计算机访问权限。 例如，可以为某一特定安全策略创建一个安全组。 以此方式可以一次性为所有成员赋予一组权限，而不必单独地向每个成员添加权限。 有关管理对资源的访问权限的详细信息，请参阅[使用 Azure Active Directory 组管理对资源的访问权限](active-directory-manage-groups.md)。
               - **Office 365**。 通过向成员赋予对共享邮箱、日历、文件、SharePoint 站点等的访问权限，提供协作机会。 你也可以通过此选项向组织外部的人员赋予对组的访问权限。 有关 Office 365 组的详细信息，请参阅[了解 Office 365 组](https://support.office.com/article/learn-about-office-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2)。

   - **组名称（必填）。** 添加组名称，可以使用容易记住以及具有某种意义的名称作为组名称。 将执行检查以确定该名称是否已用于另一个组。 如果该名称已在使用中，为避免重复命名，系统将要求你修改组的名称。

   - **组描述。** 向组添加说明（可选操作）。

   - **成员身份类型（必填）。** 选择预定义的成员身份类型。 这包括：- **已分配**。 允许添加特定用户成为该组成员并获得独特权限。 为了更好地阐述本文，我们使用此选项。

1. 选择“创建”  。

    随即将创建组，该组已准备就绪，可供添加成员。

1. 从“组”页面选择“成员”区域，然后从“选择成员”页面中开始搜索要添加到组的成员    。

    ![在组创建过程中选择你的组成员](./media/active-directory-groups-create-azure-portal/select-members-create-group.png)

1. 完成添加成员后，选择“选择”  。

    “组概述”页已更新，可显示当前添加到组的成员数  。

    ![突出显示成员数的“组概述”页](./media/active-directory-groups-create-azure-portal/group-overview-blade-number-highlight.png)

## <a name="turn-on-or-off-welcome-email"></a>打开或关闭欢迎电子邮件

创建任何新的 Office 365 组时，会向添加到该组的所有用户发送欢迎通知。 

## <a name="next-steps"></a>后续步骤
现在已添加一个组和至少一个用户，你可以：

- [查看组和成员](active-directory-groups-view-azure-portal.md)

- [管理组成员身份](active-directory-groups-membership-azure-portal.md)

- [编辑组设置](active-directory-groups-settings-azure-portal.md)

- [使用组管理对资源的访问权限](active-directory-manage-groups.md)

- [使用 PowerShell 命令管理组](../users-groups-roles/groups-settings-v2-cmdlets.md)

- [将 Azure 订阅关联或添加到 Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

<!-- Update_Description: wording update -->