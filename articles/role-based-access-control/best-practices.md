---
title: Azure RBAC 最佳做法
description: 使用 Azure 基于角色的访问控制 (Azure RBAC) 的最佳做法。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2020
ms.author: v-junlch
ms.reviewer: bagovind
ms.openlocfilehash: e2333567595a9c8074d6b3a615d122547fd455c9
ms.sourcegitcommit: e3512c5c2bbe61704d5c8cbba74efd56bfe91927
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82272060"
---
# <a name="best-practices-for-azure-rbac"></a>Azure RBAC 最佳做法

本文介绍了使用 Azure 基于角色的访问控制 (Azure RBAC) 的一些最佳做法。 这些最佳做法源自我们的 Azure RBAC 经验和客户经验。

## <a name="only-grant-the-access-users-need"></a>仅授予用户所需的访问权限

使用 Azure RBAC，可以在团队中实现职责分离，仅向用户授予他们执行作业所需的访问权限。 请勿向每个人授予 Azure 订阅或资源的无限制权限，只能允许他们在特定的范围执行某些操作。

规划访问控制策略时，最佳做法是授予用户完成工作所需的最低权限。 下图显示了与 RBAC 使用有关的建议模式。

![RBAC 和最小特权](./media/best-practices/rbac-least-privilege.png)

有关如何添加角色分配的信息，请参阅[添加或删除角色分配](role-assignments-portal.md)。

## <a name="limit-the-number-of-subscription-owners"></a>限制订阅所有者的数量

最多只能有 3 个订阅所有者，这样可降低被入侵的所有者做出违规行为的可能性。 可以在 Azure 安全中心内监视此建议措施。 

## <a name="use-azure-ad-privileged-identity-management"></a>使用 Azure AD Privileged Identity Management

为了保护特权帐户免受恶意网络攻击，可以使用 Azure Active Directory Privileged Identity Management (PIM) 来降低权限的暴露时间，并通过报表和警报增加对使用的可见性。 PIM 提供对 Azure AD 和 Azure 资源的实时特权访问权限，有助于保护特权帐户。 访问可能有时间限制，在超过时限后会自动撤销特权。 

有关详细信息，请参阅[什么是 Azure AD Privileged Identity Management？](../active-directory/privileged-identity-management/pim-configure.md)。

## <a name="next-steps"></a>后续步骤

- [排查 Azure RBAC 问题](troubleshooting.md)

