---
title: .create table - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .create table。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/06/2020
ms.date: 07/01/2020
ms.openlocfilehash: a32cd1defb78913b3517b7fcb8f8e20e625450da
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470352"
---
# <a name="create-table"></a>.create table

创建新的空表。

该命令必须在特定数据库的上下文中运行。

需要[数据库用户权限](../management/access-control/role-based-authorization.md)。

**语法**

`.create` `table` _TableName_ ([columnName:columnType], ...) [`with` `(`[`docstring` `=` _Documentation_][`,` `folder` `=` *foldername*] `)`]

如果该表已存在，则此命令将返回“success”。

**示例**

```kusto
.create table MyLogs ( Level:string, Timestamp:datetime, UserId:string, TraceId:string, Message:string, ProcessId:int32 )
```

**返回输出**

以 JSON 格式返回表的架构，与下面的命令相同：

```kusto
.show table MyLogs schema as json
```

> [!NOTE]
> 若要创建多个表，请使用 [.create tables](create-tables-command.md) 命令以提高性能并降低群集负载。

## <a name="create-merge-table"></a>.create-merge table

创建新表或扩展现有表。

该命令必须在特定数据库的上下文中运行。

需要[数据库用户权限](../management/access-control/role-based-authorization.md)。

**语法**

`.create-merge` `table` _TableName_ ([columnName:columnType], ...) [`with` `(`[`docstring` `=` _Documentation_][`,` `folder` `=` *foldername*] `)`]

如果该表不存在，则其功能与“.create table”命令完全相同。

如果表 T 存在并且你发送了“.create-merge table T (<columns specification>)”命令，则：

- <columns specification> 中先前在 T 中不存在的任何列都将添加到 T 的架构的末尾。
- T 中不在 <columns specification> 中的任何列都不会从 T 中删除。
- <columns specification> 中存在于 T 中但数据类型不同的任何列都会导致命令失败。
