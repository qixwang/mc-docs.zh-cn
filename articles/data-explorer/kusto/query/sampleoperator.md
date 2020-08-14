---
title: sample 运算符 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 sample 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/18/2020
ms.date: 08/06/2020
ms.openlocfilehash: 5f2a4e47dc07bb0434e7d6afa542cdbe8d48cbf3
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841574"
---
# <a name="sample-operator"></a>sample 运算符

从输入表返回最大指定随机行数。

```kusto
T | sample 5
```

**语法**

_T_ `| sample` _NumberOfRows_

**参数**

- NumberOfRows：要返回的 T 的行数。 可以指定任何数值表达式。

**备注**

- `sample` 适用于速度而不是对值的均匀分配。 具体来说，这意味着如果在联合了两个不同大小的数据集（如 `union` 或 `join` 运算符）的运算符之后使用，它将不会产生“公平”的结果。 建议在表格引用和筛选器后立即使用 `sample`。

- `sample` 是一个非确定性运算符，每次在查询过程中对其求值时，它都会返回不同的结果集。 例如，以下查询将产生两个不同的行（即使其中一个预期会返回相同的行两次）。

```kusto
let _data = range x from 1 to 100 step 1;
let _sample = _data | sample 1;
union (_sample), (_sample)
```

| x   |
| --- |
| 83  |
| 3   |

为确保在上面的示例中 `_sample` 计算一次，可以使用 [materialize()](./materializefunction.md) 函数：

```kusto
let _data = range x from 1 to 100 step 1;
let _sample = materialize(_data | sample 1);
union (_sample), (_sample)
```

| x   |
| --- |
| 34  |
| 34  |

**提示**

- 如果要对一定百分比的数据（而不是指定的行数）进行采样，则可以使用

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents | where rand() < 0.1
```

- 如果要对项而不是行进行采样（例如对 10 个 ID 进行采样并获取这些 ID 的所有行），则可以将 [`sample-distinct`](./sampledistinctoperator.md) 与 `in` 运算符结合使用。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
let sampleEpisodes = StormEvents | sample-distinct 10 of EpisodeId;
StormEvents
| where EpisodeId in (sampleEpisodes)
```
