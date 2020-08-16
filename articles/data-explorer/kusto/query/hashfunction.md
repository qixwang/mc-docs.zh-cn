---
title: hash() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 hash()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 5d4c7247fb042e3ddd164b27ae10978a4c6a0254
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841442"
---
# <a name="hash"></a>hash()

返回输入值的哈希值。

**语法**

`hash(`*source* [`,` *mod*]`)`

**参数**

* *source*：要进行哈希处理的值。
* mod：一个可选模块值，在应用于哈希结果后输出值将在 `0` 到 mod - 1 之间

**返回**

给定标量的哈希值，对给定 mod 值（如果已指定）取模。

> [!WARNING]
> 用于计算哈希的算法是 xxhash。
> 此算法将来可能会更改，唯一的保证是：在单个查询中，对这个方法的所有调用都使用相同的算法。
> 因此，建议不要将 `hash()` 的结果存储在表中。 如果需要持久化哈希值，请改用 [hash_sha256()](./sha256hashfunction.md) 或 [hash_md5()](./md5hashfunction.md)。 请注意，计算这些函数比 `hash()` 更复杂。

**示例**

```kusto
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```

下面的示例使用哈希函数对 10% 的数据运行查询。假设值是均匀分布的（在本示例中，值为 StartTime 值），则使用哈希函数对数据采样很有帮助。

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents 
| where hash(StartTime, 10) == 0
| summarize StormCount = count(), TypeOfStorms = dcount(EventType) by State 
| top 5 by StormCount desc
```
