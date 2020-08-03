---
title: .create-merge tables - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .create-merge tables 命令。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: 9d12becf47c18fc65b2c586be8d58b4408dca9e7
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470353"
---
# <a name="create-merge-tables"></a>.create-merge 表

允许你在特定数据库的上下文中，通过单个批量操作创建和扩展现有表的架构。

> [!NOTE]
> 需要[数据库用户权限](../management/access-control/role-based-authorization.md)。
> 需要[表管理员权限](../management/access-control/role-based-authorization.md)来扩展现有表。

**语法**

`.create-merge` `tables` _TableName1_ ([columnName:columnType], ...) [`,` _TableName2_ ([columnName:columnType], ...) ... ]

- 将创建不存在的指定表。
- 已存在的指定表将扩展其架构。
  - 不存在的列将添加到现有表架构的末尾。
  - 此命令中未指定的现有列不会从现有表的架构中删除。
  - 在命令中使用与现有表架构中的数据类型不同的数据类型指定的现有列将导致失败。 不会创建或扩展任何表。

**示例**

```kusto
.create-merge tables
  MyLogs (Level:string, Timestamp:datetime, UserId:string, TraceId:string, Message:string, ProcessId:int32),
  MyUsers (UserId:string, Name:string)
```

**返回输出**

| TableName | DatabaseName  | 文件夹 | DocString |
| --------- | ------------- | ------ | --------- |
| MyLogs    | TopComparison |        |           |
| MyUsers   | TopComparison |        |           |
