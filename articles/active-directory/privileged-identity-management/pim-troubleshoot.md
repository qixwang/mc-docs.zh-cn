---
title: 排查 Privileged Identity Management 问题 - Azure Active Directory | Microsoft Docs
description: 了解如何解决 Azure AD Privileged Identity Management (PIM) 中角色的系统错误。
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 03/25/2020
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70384f2d7e81749fe13c8d8b2bb0534d23507d14
ms.sourcegitcommit: 6568c59433d7e80ab06e9fe76d4791f761ed6775
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80243188"
---
# <a name="troubleshoot-a-problem-with-privileged-identity-management"></a>排查 Privileged Identity Management 问题

是否在使用 Azure Active Directory (Azure AD) 中的 Privileged Identity Management (PIM) 时遇到问题？ 下面的信息可帮助你将一切复原。

## <a name="access-to-azure-resources-denied"></a>拒绝对 Azure 资源的访问

### <a name="problem"></a>问题

作为 Azure 资源的活动所有者或用户访问管理员，可以查看 Privileged Identity Management 中的资源，但不能执行任何操作，例如进行符合条件的分配或从资源概述页查看角色分配列表。 其中任何操作都会导致授权错误。

### <a name="cause"></a>原因

当从订阅中意外删除 PIM 服务主体的“用户访问管理员”角色时，可能会发生此问题。 为了使 Privileged Identity Management 服务能够访问 Azure 资源，应始终为 MS-PIM 服务主体分配 Azure 订阅上的[“用户访问管理员”角色](../../role-based-access-control/built-in-roles.md#user-access-administrator)。

### <a name="resolution"></a>解决方法

在订阅级别将“用户访问管理员”角色分配给 Privileged identity Management 服务主体名称 (MS-PIM)。 此分配应允许 Privileged identity Management 服务访问 Azure 资源。 根据你的要求，可以在管理组级别或订阅级别分配角色。 有关服务主体的详细信息，请参阅[将应用程序分配给角色](/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application)。

## <a name="next-steps"></a>后续步骤

- [使用 Privileged Identity Management 的许可要求](subscription-requirements.md)
- [部署 Privileged Identity Management](pim-deployment-plan.md)

