---
title: 溢出 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的溢出。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: 6dbc15046a7b6b00ff19fe21ab333feea5d6ba6b
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226233"
---
# <a name="overflows"></a>溢出

如果计算结果对于目标类型来说太大，则会发生溢出。
溢出通常会导致[部分查询失败](partialqueryfailures.md)。

例如，下面的查询会导致溢出。

```kusto
let Weight = 92233720368547758;
range x from 1 to 3 step 1
| summarize percentilesw(x, Weight * 100, 50)
```

Kusto 的 `percentilesw()` 实现会针对“足够接近”的值将 `Weight` 表达式聚合。
在此示例中，聚合会触发溢出，因为它不适合带符号的 64 位整数。

通常，溢出是查询中“bug”的结果，因为 Kusto 使用 64 位类型进行算术计算。
最好的做法是查看错误消息，确定触发溢出的函数或聚合。 请确保输入参数的计算结果为有意义的值。
