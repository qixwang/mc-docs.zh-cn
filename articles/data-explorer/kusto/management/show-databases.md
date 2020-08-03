---
title: .show databases - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .show databases。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: eba0d22438d73aa1677d3ffb7affd18cdb450b87
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470458"
---
# <a name="show-databases"></a>.show databases

返回一个表，其中的每条记录都与该用户有权访问的群集中的某个数据库相对应。

有关显示上下文数据库属性的表，请参阅 [`.show database`](show-database.md)。

**语法**

`.show` `databases`

**输出架构**

| 列名称        | 列类型 | 说明                                                                  |
| ------------------ | ----------- | ---------------------------------------------------------------------------- |
| DatabaseName       | `string`    | 附加到群集的数据库的名称                             |
| PersistentStorage  | `string`    | 数据库的持久存储“根”。 仅供内部使用         |
| 版本            | `string`    | 数据库的版本。 仅供内部使用                           |
| IsCurrent          | `bool`      | 此数据库是否为请求的数据库上下文                 |
| DatabaseAccessMode | `string`    | `ReadWrite`、`ReadOnly`、`ReadOnlyFollowing` 或 `ReadWriteEphemeral` 之一 |
| PrettyName         | `string`    | 数据库的易记名称（如果有）                                      |
| ReservedSlot1      | `bool`      | 保留。 仅供内部使用                                              |
| 数据库 ID         | `guid`      | 数据库的全局唯一标识符。 仅供内部使用         |
