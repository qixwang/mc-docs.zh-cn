---
title: 广播联接 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的广播联接。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: eec1b4ac481910003da851c0303719d6a4f55d7a
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509222"
---
# <a name="broadcast-join"></a>广播联接

如今，常规联接在单个集群节点上执行。
广播联接是一种联接的执行策略，该策略分布在各群集节点上。 在联接的左侧较小（最多 100,000 条记录）时，此策略很有用。 在这种情况下，广播联接比常规联接的性能更高。

如果联接的左侧是一个小型数据集，可使用以下语法 (hint.strategy = broadcast) 在广播模式下运行联接：

```kusto
lookupTable 
| join hint.strategy = broadcast (factTable) on key
```

在联接后跟其他运算符（如 `summarize`）的情况下，性能改进将更加明显。 例如，在以下查询中：

```kusto
lookupTable 
| join hint.strategy = broadcast (factTable) on Key
| summarize dcount(Messages) by Timestamp, Key
```