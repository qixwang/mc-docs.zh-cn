---
title: 应用程序列表中意料之外的应用程序 | Microsoft Docs
description: 如何查看租户中的全部应用程序，并了解应用程序如何显示在“企业应用程序”下的“所有应用程序”列表中
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/11/2017
ms.date: 07/04/2019
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5827bc431475b032138fbf9fc505932d3d8af545
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67568599"
---
# <a name="unexpected-application-in-my-applications-list"></a>应用程序列表中意料之外的应用程序

本文介绍应用程序如何显示在“企业应用程序”  下的“所有应用程序”  列表中。 

## <a name="how-to-see-all-applications-in-your-tenant"></a>如何查看租户中的全部应用程序

若要查看租户中的所有应用程序，需要使用“筛选器”  控件，在“所有应用程序”  列表下显示“所有应用程序”  。 执行以下步骤：

1. 打开 [**Azure 门户**](https://portal.azure.cn/)，并以“全局管理员”或“共同管理员”身份登录。  

2. 选择“Azure Active Directory”  。

4. 在 Azure Active Directory 的左侧导航菜单中，单击“企业应用程序”  。

5. 单击“所有应用程序”  ，查看所有应用程序的列表。

6. 在“所有应用程序列表”  顶部，单击以使用“筛选器”  控件。

7. 在“筛选器”  窗格上，将“显示”  选项设为“所有应用程序”  。

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>为什么在所有应用程序列表中出现某个特定应用程序？

筛选为“所有应用程序”  时，“所有应用程序列表”   会显示租户中的每个服务主体对象。 服务主体对象以多种方式出现在此列表中：

1. 注册或登录与 Azure Active Directory 集成的<sup></sup>第三方应用程序时。 [Smartsheet](https://app.smartsheet.com/b/home) 或 [DocuSign](https://www.docusign.net/member/MemberLogin.aspx) 就是一个示例。

2. 注册第一方应用程序或者将用户或组的许可证添加到其中时，例如 [Microsoft Office 365](https://products.office.com/)

3. 当通过使用[应用程序注册表](/active-directory/active-directory-app-registration)创建以自定义方式开发的应用程序，来添加新应用程序注册之时

6. 添加正在使用 Visual Studio 的 [ASP.net 身份验证方法](https://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)或[连接的服务](https://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)开发的应用程序时

6. 使用 [Azure AD PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-adv2?view=azureadps-2.0)创建服务主体对象时

8. 以管理员身份[同意某应用程序](/active-directory/develop/active-directory-devhowto-multi-tenant-overview)使用租户中的数据时

9. [用户同意某应用程序](/active-directory/develop/active-directory-devhowto-multi-tenant-overview)使用租户中的数据时

9. 启用某些在租户中存储数据的服务时。 相应的一个示例是密码重置，密码重置是作为服务主体进行建模的，以便安全存储密码重置策略。

要详细深入地了解如何将应用添加到目录，请参阅[如何以及为何将应用程序添加到 Azure AD](/active-directory/develop/active-directory-how-applications-are-added)。

## <a name="i-want-to-remove-a-specific-users-assignment-to-an-application"></a>我要删除到应用程序的特定用户分配

若要删除应用程序的用户，请按照[在 Azure Active Directory 中删除企业应用中的用户](/active-directory/active-directory-coreapps-remove-assignment-azure-portal)一文中所列的步骤进行操作。

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>我要禁用每个用户对应用程序的所有访问权限

若要禁用所有用户对应用程序的登录，请遵循[在 Azure Active Directory 中对企业应用禁用用户登录](/active-directory/active-directory-coreapps-disable-app-azure-portal)一文中所列出的步骤。

## <a name="i-want-to-delete-an-application-entirely"></a>我要彻底删除应用程序

若要**删除应用程序**，请执行以下步骤：

1. 打开 [**Azure 门户**](https://portal.azure.cn/)，并以“全局管理员”或“共同管理员”身份登录。  

2. 在左侧主导航菜单顶部单击“所有服务”  ，打开“Azure Active Directory 扩展”  。

3. 在筛选器搜索框中键入“Azure Active Directory”  ，选择“Azure Active Directory”  项。

4. 在 Azure Active Directory 的左侧导航菜单中，单击“企业应用程序”  。

5. 单击“所有应用程序”  ，查看所有应用程序的列表。

   * 如果未看到要在此处显示的应用程序，请使用“所有应用程序列表”  顶部的“筛选器”  控件，并将“显示”  选项设置为“所有应用程序”  。

6. 选择要删除的应用程序。

7. 加载应用程序后，在应用程序顶部的“概述”  窗格中，单击“删除”  图标。


<!-- Update_Description: update metedata properties -->
