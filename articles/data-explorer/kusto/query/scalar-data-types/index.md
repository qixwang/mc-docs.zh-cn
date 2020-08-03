---
title: 标量数据类型 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的标量数据类型。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 01/27/2020
ms.date: 07/31/2020
ms.openlocfilehash: 78ac7e891145fa224ee2eb49abcb9cbb9aa324e1
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509240"
---
# <a name="scalar-data-types"></a>标量数据类型

每个数据值（例如表达式的值或函数的参数）都有一个**数据类型**。 数据类型为**标量数据类型**（一种内置的预定义类型，已列在下面）或**用户定义的记录**（一系列已排序的名称/标量数据类型对，例如表的某个行的数据类型）。

Kusto 提供一组系统数据类型，这些类型定义了可以与 Kusto 配合使用的所有类型的数据。

> [!NOTE]
> Kusto 不支持用户定义的数据类型。

下表列出了 Kusto 支持的数据类型、其他可以用来引用它们的别名，以及大致等效的 .NET Framework 类型。

| 类型       | 其他名称   | 等效 .NET 类型              | gettype()   |
| ---------- | -------------------- | --------------------------------- | ----------- |
| `bool`     | `boolean`            | `System.Boolean`                  | `int8`      |
| `datetime` | `date`               | `System.DateTime`                 | `datetime`  |
| `dynamic`  |                      | `System.Object`                   | `array` 或 `dictionary`，或者任何其他值 |
| `guid`     | `uuid`, `uniqueid`   | `System.Guid`                     | `guid`      |
| `int`      |                      | `System.Int32`                    | `int`       |
| `long`     |                      | `System.Int64`                    | `long`      |
| `real`     | `double`             | `System.Double`                   | `real`      |
| `string`   |                      | `System.String`                   | `string`    |
| `timespan` | `time`               | `System.TimeSpan`                 | `timespan`  |
| `decimal`  |                      | `System.Data.SqlTypes.SqlDecimal` | `decimal`   |

所有非字符串数据类型都包括一个特殊的“null”值，它表示缺少数据或数据不匹配。 例如，尝试将字符串 `"abc"` 引入 `int` 列中会生成此值。
无法以显式方式将此值具体化，但可使用 `isnull()` 函数来检测某个表达式的求值是否为此值。

> [!WARNING]
> 对 `guid` 类型的支持尚不完全。
> 强烈建议相关团队改用类型为 `string` 的值。
