---
title: 删除 Azure AD 组织（租户）- Azure Active Directory | Microsoft Docs
description: 说明如何使 Azure AD 组织（租户）为删除做好准备，包括自助服务组织
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 07/06/2020
ms.author: v-junlch
ms.reviewer: addimitu
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd9e38b0166019b25ba97f02f4ca0d1084425d3b
ms.sourcegitcommit: 92b9b1387314b60661f5f62db4451c9ff2c49500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86165036"
---
# <a name="delete-a-tenant-in-azure-active-directory"></a>在 Azure Active Directory 中删除一个租户

删除 Azure AD 组织（租户）时，也会删除包含在该组织中的所有资源。 在删除前要最大程度地减少与组织关联的资源，使组织为删除做好准备。 只有 Azure Active Directory (Azure AD) 全局管理员可从门户中删除 Azure AD 组织。

## <a name="prepare-the-organization"></a>准备组织

除非 Azure AD 中的组织通过了若干项检查，否则无法将其删除。 这些检查可以降低删除 Azure AD 组织后对用户访问造成负面影响（例如，影响登录 Office 365 或访问 Azure 中的资源）的风险。 例如，如果意外删除了与订阅关联的组织，则用户将无法访问该订阅的 Azure 资源。 需检查以下情况：

* 除了一位要删除组织的全局管理员外，Azure AD 组织（租户）中不得有任何用户。 在删除组织之前，必须先删除任何其他用户。 如果用户是从本地同步的，则必须先关闭同步，并且必须使用 Azure 门户或 Azure PowerShell cmdlet 从云组织中删除这些用户。
* 该组织中不能有任何应用程序。 在删除组织之前，必须先删除任何应用程序。
* 不能有任何多重身份验证提供程序链接到该组织。
* 该组织不能有与之关联的任何 Microsoft Online Services（例如 Azure、Office 365 或 Azure AD Premium）订阅。 例如，如果在 Azure 中创建了一个默认 Azure AD 组织，并且 Azure 订阅仍然依赖于此组织进行身份验证，则不能删除此组织。 类似地，如果其他用户已将订阅与某个组织相关联，则无法删除该组织。

## <a name="delete-the-organization"></a>删除组织

1. 使用一个其身份为组织全局管理员的帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 选择“Azure Active Directory” 。

3. 切换到要删除的组织。
  
   ![删除前确认组织](./media/directory-delete-howto/delete-directory-command.png)

4. 选择“删除租户”。
  
   ![选择命令以删除组织](./media/directory-delete-howto/delete-directory-list.png)

5. 如果组织未通过一个或多个检查，系统会提供相关链接，介绍如何通过检查的详细信息。 通过所有检查后，请选择“删除”，此过程结束。

## <a name="next-steps"></a>后续步骤

[Azure Active Directory 文档](/active-directory/)

<!-- Update_Description: wording update -->
