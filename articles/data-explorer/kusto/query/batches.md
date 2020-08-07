---
title: 批处理 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的批处理。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 3bbb8dcf9c1666d3980d0222935d9f93426dee18
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509217"
---
# <a name="batches"></a>批处理

一个查询可以包含多个表格表达式语句，只要以分号 (`;`) 字符分隔它们即可。 这样，该查询会返回多个表格结果。 结果由表格表达式语句产生，并根据查询文本中语句的顺序进行排序。

例如，以下查询产生两个表格结果。 然后，用户代理工具可以显示这些结果以及与每个结果相关联的适当名称（分别为 `Count of events in Florida` 和 `Count of events in Guam`）。

```kusto
StormEvents | where State == "FLORIDA" | count | as ['Count of events in Florida'];
StormEvents | where State == "GUAM" | count | as ['Count of events in Guam']
```

对于多个子查询共享一个通用计算的场景（例如仪表板），批处理很有用。 如果通用计算很复杂，请使用 [materialize() 函数](./materializefunction.md)，并构造查询以使其仅执行一次：

```kusto
let m = materialize(StormEvents | summarize n=count() by State);
m | where n > 2000;
m | where n < 10
```

说明：
* 优先考虑批处理和 [`materialize`](materializefunction.md) 而不是使用 [fork 运算符](forkoperator.md)。
