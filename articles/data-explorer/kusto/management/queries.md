---
title: 查询管理 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的查询管理。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/23/2020
ms.date: 07/01/2020
ms.openlocfilehash: a9c04c1efd3e7c3dec91320ad42fe57d161bf41e
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470338"
---
# <a name="queries-management"></a>查询管理

## <a name="show-queries"></a>.show 查询

`.show` `queries` 命令返回已达到最终状态的查询列表，并且调用该命令的用户有权查看：

- [数据库管理员或数据库监视员](../management/access-control/role-based-authorization.md)可以查看在他们的数据库上调用的任何命令。
- 其他用户只能看到由他们调用的查询。

**语法**

`.show` `queries`

## <a name="show-running-queries"></a>.show 正在运行的查询

`.show` `running` `queries` 命令返回用户或另一个用户或所有用户当前正在执行的查询的列表。

**语法**

```kusto
.show running queries
```

- (1) 返回调用用户当前正在执行的查询（需要读取访问权限）。

## <a name="cancel-query"></a>.cancel 查询

`.cancel` `query` 命令开始尽力尝试取消同一用户先前启动的特定查询。

- 集群管理员可以取消任何正在运行的查询。
- 数据库管理员可以取消对其具有管理员访问权限的数据库调用的任何正在运行的查询。
- 其他用户只能取消他们启动的查询。

**语法**

`.cancel` `query` _ClientRequestId_

- “ClientRequestId”是原始查询 ClientRequestId 字段的值，形式为 `string` 文本。

**示例**

```kusto
.cancel query "KE.RunQuery;8f70e9ab-958f-4955-99df-d2a288b32b2c"
```
