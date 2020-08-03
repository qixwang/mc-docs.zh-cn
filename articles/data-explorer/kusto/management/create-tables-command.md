---
title: .create tables - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .create tables。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: c18de5441c8d3491f5360bb243a07b10556755e9
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470430"
---
# <a name="create-tables"></a>.create tables

创建新的空表作为批量操作。

该命令必须在特定数据库的上下文中运行。

需要[数据库用户权限](../management/access-control/role-based-authorization.md)。

**语法**

`.create` `tables` TableName1 ([columnName:columnType], ...) [`,` TableName2 ([columnName:columnType], ...) ... ] 

如果已经存在任何表，命令将返回成功。

**示例**

```kusto
.create tables
  MyLogs (Level:string, Timestamp:datetime, UserId:string, TraceId:string, Message:string, ProcessId:int32),
  MyUsers (UserId:string, Name:string)
```
