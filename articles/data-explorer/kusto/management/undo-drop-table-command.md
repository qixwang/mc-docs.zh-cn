---
title: .undo 删除表 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .undo 删除表。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: 52147d9d3e22d3cbe26e259a40c16698216346f6
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470537"
---
# <a name="undo-drop-table"></a>.undo drop table

`.undo` `drop` `table` 命令将删除表操作还原为特定的数据库版本。

**语法**

`.undo` `drop` `table` _TableName_ [`as` *NewTableName*] `version=v` _DB_MajorVersion.DB_MinorVersion_

该命令必须在数据库上下文中执行。

**返回**

此命令：

- 返回原始表盘区列表
- 为每个盘区指定盘区包含的记录数
- 恢复操作成功或失败都返回
- 如果相关，则返回失败原因。

| ExtentId                             | NumberOfRecords | 状态                   | FailureReason                                                                                                                  |
| ------------------------------------ | --------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| ef296c9e-d75d-44bc-985c-b93dd2519691 | 100             | 已恢复                |
| 370b30d7-cf2a-4997-986e-3d05f49c9689 | 1000            | 已恢复                |
| 861f18a5-6cde-4f1e-a003-a43506f9e8da | 855             | 无法恢复盘区 | 盘区容器：找不到 4b47fd84-c7db-4cfb-9378-67c1de7bf154，此盘区已从存储中删除，无法恢复 |

**示例**

```kusto
// Recover TestTable table to database version 24.3
.undo drop table TestTable version="v24.3"
```

```kusto
// Recover TestTable table to database version 10.3 with new table name, NewTestTable (can be used if a table with the same name was already created since the drop)
.undo drop table TestTable as NewTestTable version="v10.3"
```

**如何查找所需的数据库版本**

可以使用 `.show` `journal` 命令在执行删除操作之前找到数据库版本：

```kusto
.show database TestDB journal | where Event == "DROP-TABLE" and EntityName == "TestTable" | project OriginalEntityVersion
```

| OriginalEntityVersion |
| --------------------- |
| v24.3                 |

**限制**

如果对此数据库执行了 Purge 命令，则不能将 undo drop table 命令执行到清除执行之前的版本。

仅当尚未达到数据库所在盘区容器的硬删除期时，才可以恢复盘区。

该命令需要[数据库管理员权限](../management/access-control/role-based-authorization.md)。
