---
title: Kusto RestrictedViewAccess 策略控制查询 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 RestrictedViewAccess 策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 07/01/2020
ms.openlocfilehash: 7dbea999199434fe0187945f5fa633fa35cff2f5
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470332"
---
# <a name="restrictedviewaccess-policy"></a>RestrictedViewAccess 策略

RestrictedViewAccess 是一个可为数据库的表启用的可选策略。

当为表启用此策略时，表中的数据只能由在数据库中具有 [UnrestrictedViewer](../management/access-control/role-based-authorization.md) 角色的主体查询。
未注册 [UnrestrictedViewer](../management/access-control/role-based-authorization.md) 数据库级角色的任何主体都无法查询表中的数据。 即使是表/数据库/群集的管理员，如果未注册也无法查询。

[UnrestrictedViewer](../management/access-control/role-based-authorization.md) 角色提供可查看数据库中启用了该策略的所有表的相应权限。
当前主体（数据库管理员/用户/查看者）已获得查询数据库的授权。
添加或删除主体可以由 [DatabaseAdmin](../management/access-control/role-based-authorization.md) 完成。

> [!NOTE]
> 无法在启用[行级别安全性策略](./rowlevelsecuritypolicy.md)的表上配置 RestrictedViewAccess 策略。

有关详细信息，请参阅用于管理 [RestrictedViewAccess 策略](../management/restrictedviewaccess-policy.md)的控制命令。
