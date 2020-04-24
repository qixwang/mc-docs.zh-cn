---
title: 教程 - 使用 RBAC 和 Azure 门户授予用户对 Azure 资源的访问权限
description: 本教程介绍如何使用 Azure 门户中的基于角色的访问控制 (RBAC) 授予用户对 Azure 资源的访问权限。
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/04/2019
ms.author: v-junlch
ms.openlocfilehash: b8a493594e6b5e872e27683dacf2535e0ce9f6a6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884920"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>教程：使用 RBAC 和 Azure 门户授予用户对 Azure 资源的访问权限

可以通过[基于角色的访问控制 (RBAC)](overview.md) 管理对 Azure 资源的访问权限。 在本教程中，你将授权用户在某个资源组中创建和管理虚拟机。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 在资源组范围内为用户授予访问权限
> * 删除访问权限

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

## <a name="sign-in-to-azure"></a>登录 Azure

通过 https://portal.azure.cn 登录到 Azure 门户。

## <a name="create-a-resource-group"></a>创建资源组

1. 在导航列表中，单击“资源组”  。

1. 单击“添加”以打开“资源组”边栏选项卡。  

   ![添加新的资源组](./media/quickstart-assign-role-user-portal/resource-group.png)

1. 输入 **rbac-quickstart-resource-group** 作为**资源组名称**。

1. 选择订阅和位置。

1. 单击“创建”  以创建资源组。

1. 单击“刷新”  以刷新资源组的列表。

   新资源组会显示在资源组列表中。

   ![资源组列表](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>授予访问权限

在 RBAC 中，若要授予访问权限，请创建角色分配。

1. 在“资源组”列表中，单击这个新的 **rbac-quickstart-resource-group** 资源组。 

1. 单击“访问控制(IAM)”  。

1. 单击“角色分配”  选项卡以查看当前的角色分配列表。

   ![资源组的“访问控制(IAM)”边栏选项卡](./media/quickstart-assign-role-user-portal/access-control.png)

1. 单击“添加” > “添加角色分配”以打开“添加角色分配”窗格。  

   如果没有分配角色的权限，则将禁用“添加角色分配”选项。

   ![添加菜单](./media/role-assignments-portal/add-menu.png)

   ![“添加角色分配”窗格](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. 在“角色”下拉列表中选择“虚拟机参与者”。  

1. 在“选择”  列表中，选择你自己或另一用户。

1. 单击“保存”，创建角色分配  。

   片刻之后，系统会在 rbac-quickstart-resource-group 资源组范围为该用户分配“虚拟机参与者”角色。

   ![“虚拟机参与者”角色的分配](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>删除访问权限

在 RBAC 中，若要删除访问权限，请删除角色分配。

1. 在角色分配列表中，在具有“虚拟机参与者”角色的用户旁边添加复选标记。

1. 单击“删除”。 

   ![“删除角色分配”消息](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. 在显示的“删除角色分配”消息中，单击“是”。 

## <a name="clean-up"></a>清理

1. 在导航列表中，单击“资源组”  。

1. 单击 **rbac-quickstart-resource-group**，打开资源组。

1. 单击“删除资源组”  以删除该资源组。

   ![删除资源组](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. 在“是否确实要删除”边栏选项卡上，  键入资源组名称：**rbac-quickstart-resource-group**。

1. 单击“删除”  以删除该资源组。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [教程：使用 RBAC 和 Azure PowerShell 授予用户对 Azure 资源的访问权限](tutorial-role-assignments-user-powershell.md)


<!-- Update_Description: update metedata properties -->