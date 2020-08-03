---
title: .drop table 和 .drop tables - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .drop table 和 .drop tables。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/18/2020
ms.date: 07/01/2020
ms.openlocfilehash: 8a8bd438158c9cd9d4cd957fcacc964d90a5096e
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470386"
---
# <a name="drop-table-and-drop-tables"></a>.drop table 和 .drop tables

从数据库中删除一个或多个表。

需要[表管理员权限](../management/access-control/role-based-authorization.md)。

> [!NOTE]
> `.drop` `table` 命令仅软删除数据。 也就是说，无法查询数据，但仍可从持久存储中恢复。 根据在将数据引入到表中时会生效的[保留策略](../management/retentionpolicy.md)中的 `recoverability` 属性来硬删除基础存储项目。

**语法**

`.drop` `table` _TableName_ [`ifexists`]

`.drop` `tables` (_TableName1_, _TableName2_,..) [ifexists]

> [!NOTE]
> 如果指定 `ifexists`，那当存在不存在的表时，命令不会失败。

**示例**

```kusto
.drop table CustomersTable ifexists
.drop tables (ProductsTable, ContactsTable, PricesTable) ifexists
```

**返回**

此命令返回数据库中剩余表的列表。

| 输出参数 | 类型   | 说明                             |
| ---------------- | ------ | --------------------------------------- |
| TableName        | 字符串 | 表的名称。                  |
| DatabaseName     | String | 表所属的数据库。 |
