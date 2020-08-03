---
title: RowOrder 策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 RowOrder 策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/24/2020
ms.date: 07/01/2020
ms.openlocfilehash: ce26d031d2e3e6ff3eb40df57d2c4cb78f866f5e
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470423"
---
# <a name="roworder-policy"></a>RowOrder 策略

本文介绍用于创建和更改[行顺序策略](../management/roworderpolicy.md)的控制命令。

## <a name="show-roworder-policy"></a>显示 RowOrder 策略

```kusto
.show table <table_name> policy roworder

.show table * policy roworder
```

## <a name="delete-roworder-policy"></a>删除 RowOrder 策略

```kusto
.delete table <table_name> policy roworder
```

## <a name="alter-roworder-policy"></a>更改 RowOrder 策略

```kusto
.alter table <table_name> policy roworder (<row_order_policy>)

.alter tables (<table_name> [, ...]) policy roworder (<row_order_policy>)

.alter-merge table <table_name> policy roworder (<row_order_policy>)
```

**示例**

下面的示例将 `TenantId` 列上的行顺序策略（升序）设置为主键，将 `Timestamp` 列上的行顺序策略（升序）设置为辅助键。 然后查询策略。

```kusto
.alter table events policy roworder (TenantId asc, Timestamp desc)

.alter tables (events1, events2, events3) policy roworder (TenantId asc, Timestamp desc)

.show table events policy roworder
```

| TableName | RowOrderPolicy                 |
| --------- | ------------------------------ |
| events    | (TenantId asc, Timestamp desc) |
