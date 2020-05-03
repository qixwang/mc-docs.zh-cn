---
title: 如何管理 Azure AD 中的非活动用户帐户 | Microsoft Docs
description: 了解如何检测和处理 Azure AD 中已过时的用户帐户
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/21/2020
ms.author: v-junlch
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6469f0825d5d3a99382af73bfd86d3fda80289b
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82127179"
---
# <a name="how-to-manage-inactive-user-accounts-in-azure-ad"></a>如何：管理 Azure AD 中的非活动用户帐户

在大型环境中，当员工离开组织时，并非总会删除其用户帐户。 作为 IT 管理员，你希望检测并处理这些过时的用户帐户，因为它们会带来安全风险。

本文介绍了处理 Azure AD 中已过时用户帐户的方法。 

## <a name="what-are-inactive-user-accounts"></a>什么是非活动用户帐户？

非活动帐户是指组织成员不再需要用来访问你的资源的用户帐户。 非活动帐户的一个重要特点是有一段时间未使用它来登录到你的环境。  由于非活动帐户与登录活动密切相关，因此可以使用上次成功登录的时间戳来检测这些帐户。 

此方法的难点在于，如何定义“一段时间”对于你的环境的含义。  例如，用户可能由于休假而有一段时间未登录到环境。  在定义非活动用户帐户的时间变量时，需要考虑用户未登录到环境的所有合理原因。 在许多组织中，非活动用户帐户的时间变量为 90 到 180 天。 

根据上次成功登录时间，也许能够确定用户是否需要继续访问资源。  它可以帮助确定是仍需保留组成员身份或应用访问权限，还是可以将其删除。 对于外部用户管理，你可以了解外部用户在租户中仍处于活动状态，还是应该清理该用户。 

    
## <a name="how-to-detect-inactive-user-accounts"></a>如何检测非活动用户帐户

可以通过评估 Microsoft Graph API 的 signInActivity 资源类型公开的 lastSignInDateTime 属性，来检测非活动帐户。    使用此属性可为以下方案实现解决方案：

- **按姓名列出用户**：此方案将按姓名搜索特定的用户，这样就可以评估 lastSignInDate：`https://microsoftgraph.chinacloudapi.cn/beta/users?$filter=startswith(displayName,'markvi')&$select=displayName,signInActivity`

- **按日期列出用户**：在此方案中，你使用指定日期之前的某个 lastSignInDateTime 请求用户列表：`https://microsoftgraph.chinacloudapi.cn/beta/users?filter=signInActivity/lastSignInDateTime le 2019-06-01T00:00:00Z`






## <a name="what-you-need-to-know"></a>需要了解的事项

本部分列出了对于 lastSignInDateTime 属性你需要了解的事项。

### <a name="how-can-i-access-this-property"></a>如何访问此属性？

lastSignInDateTime 属性由 [Microsoft Graph REST API](https://docs.microsoft.com/graph/overview?view=graph-rest-beta#whats-in-microsoft-graph) 的 [signInActivity 资源类型](https://docs.microsoft.com/graph/api/resources/signinactivity?view=graph-rest-beta)公开。    

### <a name="is-the-lastsignindatetime-property-available-through-the-get-azureaduser-cmdlet"></a>是否可以通过 Get-AzureAdUser cmdlet 使用 lastSignInDateTime 属性？

否。

### <a name="what-edition-of-azure-ad-do-i-need-to-access-the-property"></a>需要使用哪个 Azure AD 版本来访问此属性？

可以在所有 Azure AD 版本中访问此属性。

### <a name="what-permission-do-i-need-to-read-the-property"></a>读取此属性需要哪些权限？

若要读取此属性，需要授予以下权限： 

- AuditLogs.Read.All
- Organisation.Read.All  


### <a name="when-does-azure-ad-update-the-property"></a>Azure AD 何时更新此属性？

每次成功完成交互式登录都会更新底层数据存储。 通常，成功的登录会在 10 分钟内显示在相关的登录报告中。
 

### <a name="what-does-a-blank-property-value-mean"></a>空白属性值的含义是什么？

若要生成 lastSignInDateTime 时间戳，需要成功登录。 由于 lastSignInDateTime 属性是一项新功能，因此，在以下情况下，lastSignInDateTime 属性的值可能是空白的：

- 用户的上次成功登录发生在此功能发布日期（2019 年 12月 1 日）之前。
- 从未使用受影响的用户帐户成功登录。

## <a name="next-steps"></a>后续步骤

* [审核 API 参考](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [登录活动报告 API 参考](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)

