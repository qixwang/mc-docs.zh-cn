---
title: 管理单元管理（预览版）- Azure Active Directory | Microsoft Docs
description: 在 Azure Active Directory 中使用管理单元获得更精细的委派权限
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
origin.date: 08/01/2019
ms.date: 11/14/2019
ms.author: v-junlch
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4a23869b255c2a6f66ed136ddc1dae73422e329a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74084794"
---
# <a name="administrative-units-management-in-azure-active-directory-preview"></a>Azure Active Directory 中的管理单元管理（预览版）

本文介绍 Azure Active Directory (Azure AD) 中的管理单元。 管理单元是一项 Azure AD 资源，可以是其他 Azure AD 资源的容器。 在此预览版中，这些资源只能是用户。 例如，管理单元范围的用户帐户管理员只能在管理单元中为用户更新配置文件信息、重置密码和分配许可证。

可以使用管理单元向一部分用户委托管理权限，并向一部分用户应用策略。 可以使用管理单元将权限委派给区域管理员或以粒度级别设置策略。

## <a name="deployment-scenario"></a>部署方案

管理单元可以用于具有独立部门的组织。 可以考虑这样一个示例：一个大型大学由互相独立的许多自治学院（商学院、工程学院等）组成，每个学院都有自己的 IT 管理员，为学院控制访问权限、管理用户以及设置策略。 中心管理员可以创建商学院的管理单元，并仅使用商学院学生和教工填充该单元。 然后，中心管理员可以将商学院的 IT 员工添加到限定范围的角色中，该角色只通过商学院管理单元中的 Azure AD 用户授予管理权限。

## <a name="license-requirements"></a>许可要求

若要使用管理单元，每个管理单元管理员都必须有 Azure Active Directory Premium 许可证。有关详细信息，请参阅 [Azure AD Premium 入门](../fundamentals/active-directory-get-started-premium.md)。

## <a name="managing-administrative-units"></a>管理管理单元

在此预览版中，若要创建和管理管理单元，唯一方法是使用 Windows PowerShell cmdlet 的 Azure Active Directory 模块，详见[使用管理单元](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

有关软件要求和安装 Azure AD 模块的详细信息，以及有关用于管理管理单元的 Azure AD 模块 cmdlet 的参考信息（包括语法、参数说明和示例），请参阅 [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0)。

## <a name="next-steps"></a>后续步骤

[Azure Active Directory 版本](../fundamentals/active-directory-whatis.md)

<!-- Update_Description: wording update -->