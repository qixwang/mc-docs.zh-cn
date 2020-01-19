---
title: 如何将用户分配到应用程序 | Microsoft Docs
description: 将用户分配到应用程序以授予访问权限
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/08/2020
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe9ed4c2b16771d34ed6a03065bb7f6644dbb2ad
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75777060"
---
# <a name="assign-users-to-an-application-in-azure-active-directory"></a>在 Azure Active Directory 中向应用程序分配用户
本文介绍如何将用户分配到 Azure Active Directory (Azure AD) 中的应用程序。 首先必须将用户分配给应用程序，然后管理员才能授予这些用户访问权限以执行以下操作：

-   通过**直接导航到应用程序的 URL**（也称为 SP 发起的登录）访问应用程序。

-   通过使用应用程序的“属性”  页上的“用户访问 URL”  （也称为 IDP 发起的登录）访问应用程序。

-   查看显示在其 [Office 365 应用程序启动器](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a)中的应用程序。

## <a name="configure-the-application-to-require-assignment"></a>将应用程序配置为需要分配

可将应用程序配置为需要分配才能访问。 若要需要分配，请执行以下操作：

1. 使用管理员帐户或以**企业应用**下应用的所有者身份登录到 Azure 门户。
2. 在主菜单中单击“所有服务”  项。
3. 选择要用于应用程序的目录。
4. 单击“企业应用程序”选项卡  。
5. 从与此目录关联的应用程序列表中选择应用程序。
6. 单击“属性”  选项卡。
7. 将“需要进行用户分配?”切换为“是”  。
8. 单击屏幕顶部的“保存”  按钮。

## <a name="assign-users"></a>分配用户

要直接将一个或多个用户分配到应用程序，请按照以下步骤操作：

1.  打开 [**Azure 门户**](https://portal.azure.cn/)并以全局管理员或非管理员应用程序所有者的身份登录  。

2.  选择“Azure Active Directory”  。

4.  在 Azure Active Directory 的左侧导航菜单中，单击“企业应用程序”  。

5.  单击“所有应用程序”  ，查看所有应用程序的列表。

    * 如果未看到要在此处显示的应用程序，请使用“所有应用程序列表”  顶部的“筛选器”  控件，并将“显示”  选项设置为“所有应用程序”  。

6.  从列表中选择要向其分配用户的应用程序。

7.  在应用程序加载后，在应用程序的左侧导航菜单中单击“用户和组”  。

8.  单击“用户和组”  列表顶部的“添加用户”  按钮，以打开“添加分配”  窗格。

9.  在“添加分配”  窗格中，单击“用户”  选择器。

10. 在“按名称或电子邮件地址搜索”  搜索框中，键入要分配的用户的**全名**或**电子邮件地址**。

11. 将鼠标悬停在列表中的“用户”  上方以显示“复选框”  。 单击用户个人资料头像或徽标旁边的复选框，将用户添加到“已选择”  列表。

12. **可选：** 如果要“添加多个用户”，请在“按名称或电子邮件地址搜索”搜索框中键入其他“全名”或“电子邮件地址”，然后单击复选框以将此用户添加到“已选择”列表      。

13. 在完成用户的选择后，单击“选择”  按钮将他们添加到要分配给应用程序的用户列表。

14. **可选：** 单击“添加分配”  窗格中的“选择角色”  选择器，选择一个角色来分配给所选用户。

15. 单击“分配”  按钮，将应用程序分配给选定用户。

经过一小段时间后，所选用户将能够使用解决方案描述部分中所述的方法启动这些应用程序。

<!-- Update_Description: wording update -->
