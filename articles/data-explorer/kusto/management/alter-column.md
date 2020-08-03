---
title: .alter column - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .alter column。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/11/2020
ms.date: 07/01/2020
ms.openlocfilehash: 56be061962ed894f13a399031ea7db6010c4b323
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470520"
---
# <a name="alter-column"></a>.alter column

更改现有表列的数据类型。

> [!WARNING]
> 更改列的数据类型后，以后的查询中将忽略该列中不属于新数据类型的任何预先存在的数据，并将其替换为 [null 值](../query/scalar-data-types/null-values.md)。 使用 `alter column` 后，即使通过使用另一个命令将列类型更改回以前的值，也无法恢复列中的数据。
> 有关在不丢失数据的情况下更改列类型的建议过程，请参阅[下文](#changing-column-type-without-data-loss)。

**语法** 

`.alter` `column` [*DatabaseName* `.`] *TableName* `.` *ColumnName* `type` `=` *ColumnNewType*
 
**示例** 

```kusto
.alter column ['Table'].['ColumnX'] type=string
```

## <a name="changing-column-type-without-data-loss"></a>在不丢失数据的情况下更改列类型

若要在保留历史数据的同时更改列类型，请创建一个具有正确类型的新表。

对于要在其中更改列类型的每个表 `T1`，请执行以下步骤：

1. 使用正确的架构（正确的列类型）创建一个表 `T1_prime`。
1. 使用 [.rename tables](rename-table-command.md) 命令交换表，这一命令允许交换表名称。

```kusto
.rename tables T_prime=T1, T1=T_prime
```

命令完成后，新的数据将流向现在具有正确类型的 `T1`，并且历史数据在 `T1_prime` 中可用。

在 `T1_prime` 数据离开保留窗口之前，需要更改触及 `T1` 的查询，以执行与 `T1_prime`的联合。