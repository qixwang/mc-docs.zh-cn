---
title: bin() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 bin()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 2cc987d9b11f787819ca869e6531267065910c2b
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509224"
---
# <a name="bin"></a>bin()

将值向下舍入到给定 bin 大小的整数倍。 

经常与 [`summarize by ...`](./summarizeoperator.md) 结合使用。
如果有一组离散值，它们会分组成较小的特定值集。

Null 值、null bin 大小或负的 bin 大小将导致 null。 

`floor()` 函数的别名。

## <a name="syntax"></a>语法

`bin(`*value*`,`*roundTo*`)`

## <a name="arguments"></a>参数

* *value*：数字、日期或时间跨度。 
* *roundTo*：“bin 大小”。 划分 *value* 的数字、日期或时间跨度。 

## <a name="returns"></a>返回

最接近以下 *value* 的 *roundTo* 倍数。  
 
```kusto
(toint((value/roundTo))) * roundTo`
```

## <a name="examples"></a>示例

表达式 | 结果
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1970-05-11 13:45:07), 1d)`|  `datetime(1970-05-11)`


以下表达式计算持续时间的直方图，其中存储桶大小为 1 秒：

```kusto
T | summarize Hits=count() by bin(Duration, 1s)
```