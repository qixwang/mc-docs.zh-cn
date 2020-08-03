---
title: 数据库管理 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的数据库管理。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/24/2020
ms.date: 07/01/2020
ms.openlocfilehash: 098eac88ab7247828970a5591eefa8945d0442b8
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470482"
---
# <a name="databases-management"></a>数据库管理

本主题介绍以下数据库控制命令：

| 命令                                             | 说明                                                                                                                       |
| --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [.show databases](show-databases.md)                | 返回一个表，其中的每条记录都与该用户有权访问的群集中的某个数据库相对应                        |
| [.show database](show-database.md)                  | 返回一个表，其中显示了上下文数据库的属性                                                                    |
| [.show cluster databases](show-cluster-database.md) | 返回一个表，其中显示已附加到群集并且可供调用此命令的用户访问的所有数据库               |
| [.alter database](alter-database.md)                | 更改数据库的易记名称                                                                                        |
| [.show database schema](show-schema-database.md)    | 返回所选数据库的结构的简单列表，其所有表和列都在单个表或 JSON 对象中 |
