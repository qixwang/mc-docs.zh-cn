---
title: 删除 Azure AD 目录 - Azure Active Directory | Microsoft Docs
description: 介绍如何准备要删除的 Azure AD 目录，包括自助服务目录
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
origin.date: 04/15/2019
ms.date: 11/14/2019
ms.author: v-junlch
ms.reviewer: addimitu
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 189e95d18e34a18a98fb5688c8eaeb454abefc22
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74084792"
---
# <a name="delete-a-directory-in-azure-active-directory"></a>删除 Azure Active Directory 中的目录

删除 Azure AD 目录时，也会删除包含在该目录中的所有资源。 准备组织时，尽量减少关联的资源，然后才能将其删除。 只有 Azure Active Directory (Azure AD) 全局管理员可以从门户中删除 Azure AD 目录。

## <a name="prepare-the-directory"></a>准备目录

在删除 Azure AD 中的某个目录之前，必须对其进行多项检查。 这些检查可以降低删除 Azure AD 目录后对用户访问造成负面影响（例如，影响用户登录 Office 365 或访问 Azure 中的资源）的风险。 例如，如果与某个订阅关联的目录被无意删除，则用户无法访问该订阅的 Azure 资源。 需检查以下条件：

* 目录中不能有用户，只能有一个负责删除该目录的全局管理员。 只有在删除所有其他用户后，才能删除该目录。 如果用户是从本地同步的，则必须先关闭同步，并且必须使用 Azure 门户或 Azure PowerShell cmdlet 从云目录中删除这些用户。
* 该目录中不能有任何应用程序。 只有在删除所有应用程序后，才能删除目录。
* 不能有任何多重身份验证提供程序关联到该目录。
* 与目录关联的任何 Microsoft Online Services（例如 Azure、Office 365 或 Azure AD Premium）不能存在任何订阅。 例如，如果在 Azure 中创建了一个默认目录，并且 Azure 订阅仍然依赖于此目录进行身份验证，则你不能删除此目录。 类似地，如果其他用户已将订阅与某个目录相关联，则你无法删除该目录。

## <a name="delete-the-directory"></a>删除目录

1. 使用一个其身份为组织全局管理员的帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 选择“Azure Active Directory”  。

3. 切换到要删除的目录。
  
   ![在删除之前确认组织](./media/directory-delete-howto/delete-directory-command.png)

4. 选择“删除目录”  。
  
   ![选择用于删除组织的命令](./media/directory-delete-howto/delete-directory-list.png)

5. 如果目录没有通过一项或多项检查，则会出现一个详细说明如何通过的链接。 通过所有检查后，选择“删除”  以完成此过程。

## <a name="next-steps"></a>后续步骤

[Azure Active Directory 文档](/active-directory/)

<!-- Update_Description: wording update -->
