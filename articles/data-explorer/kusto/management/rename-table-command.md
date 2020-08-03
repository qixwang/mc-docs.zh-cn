---
title: .rename table 和 .rename tables - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .rename table 和 .rename tables。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: 6fe9f7a5cff7ad2c864ab72a28bc8d0a2b8a06ad
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470333"
---
# <a name="rename-table-and-rename-tables"></a>.rename table 和 .rename tables

更改现有表的名称。

`.rename tables` 命令将数据库中多个表的名称作为单个事务进行更改。

需要[数据库管理员权限](../management/access-control/role-based-authorization.md)。

**语法**

`.rename` `table` OldName `to` NewName 

`.rename` `tables` NewName = OldName [`ifexists`][`,` ...] 

> [!NOTE]
>
> - OldName 是现有表的名称。 如果 OldName 不是现有表的名称，那么除非指定 `ifexists`（在这种情况下将忽略重命名命令的这一部分），否则会引发错误，并且整个命令会失败（不起作用）。
> - NewName 是现有表的新名称，之前的名称为 OldName 。
> - 如果指定了 `ifexists`，它会修改此命令的行为，以忽略重命名不存在的表的部分。

**备注**

此命令仅对范围内的数据库表起作用。
无法使用群集或数据库名称限定表名。

此命令不会创建新表，也不会删除现有表。
此命令描述的转换必须确保数据库中的表数量保持不变。

只要遵守上述规则，此命令就支持交换表名或更复杂的排列。 例如，将数据引入多个临时表，然后在单个事务中将它们与现有表交换。

**示例**

假设数据库中包含以下表：`A`、`B`、`C` 和 `A_TEMP`。
以下命令将交换 `A` 和 `A_TEMP`（现在 `A_TEMP` 和 `A` 表的名称相互对调）、将 `B` 重命名为 `NEWB` 并保持 `C` 不变。

```kusto
.rename tables A=A_TEMP, NEWB=B, A_TEMP=A
```

命令的顺序如下：

1. 创建新的临时表
1. 将现有或不存在的表替换为新表

```kusto
// Drop the temporary table if it exists
.drop table TempTable ifexists

// Create a new table
.set TempTable <| ...

// Swap the two tables
.rename tables TempTable=Table ifexists, Table=TempTable

// Drop the temporary table (which used to be Table) if it exists
.drop table TempTable ifexists
```
