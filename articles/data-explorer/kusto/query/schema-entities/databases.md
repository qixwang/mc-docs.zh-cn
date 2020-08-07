---
title: 数据库 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的数据库。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/30/2019
ms.date: 07/31/2020
ms.openlocfilehash: aebae97ef21992524f2ac3b5530a2b6032660eeb
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509238"
---
# <a name="databases"></a>数据库

Kusto 遵循用于存储数据的关系模型，其中较高级别的实体是 `database`。 

Kusto 群集可以托管多个数据库，其中每个数据库将托管其自己的[表](tables.md)集合、[存储的函数](stored-functions.md)和[外部表](externaltables.md)。
每个数据库都有自己的权限集，基于[基于角色的访问控制 (RBAC) 模型](../../management/access-control/index.md)

**说明**  

* 数据库名称不区分大小写。
* 数据库名称必须遵循[实体名称](./entity-names.md)的规则。
* 每个群集的数据库数量最大限制为 10,000。