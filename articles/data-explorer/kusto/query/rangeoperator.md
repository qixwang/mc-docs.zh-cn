---
title: range 运算符 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 range 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 685e8e20b588885a16023396d68717ce9f6e9d0c
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841513"
---
# <a name="range-operator"></a>range 运算符

生成值的单列表。

请注意，该表不含管道输入。 

**语法**

`range` columnName `from` start `to` stop `step` step

**参数**

* *columnName*：输出表中的单列名称。
* *start*：输出中的最小值。
* stop：输出中正生成的最大值（如果 step 跳过此值，则为最大值边界）。
* step：两个连续值之间的差异。 

参数必须为数字、日期或时间跨度值。 它们不能引用任何表的列。 （如需基于输入表计算范围，请使用 range 函数，可能需配合使用 mv-expand 运算符。） 

**返回**

一个包含单个列 columnName 的表，其值包括 start、start `+` step...，直至 stop    。

**示例**  

过去 7 天午夜时分的表。 bin (floor) 函数将每次时间缩减到当天的开始。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
range LastWeek from ago(7d) to now() step 1d
```

|LastWeek|
|---|
|2015-12-05 09:10:04.627|
|2015-12-06 09:10:04.627|
|...|
|2015-12-12 09:10:04.627|


一个包含单个列 `Steps` 的表，其类型是 `long`，其值是 `1`、`4` 和 `7`。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
range Steps from 1 to 8 step 3
```

下一个示例演示如何使用 `range` 运算符创建小型的临时维度表，并使用该表在源数据不具有值的位置引入零。

```kusto
range TIMESTAMP from ago(4h) to now() step 1m
| join kind=fullouter
  (Traces
      | where TIMESTAMP > ago(4h)
      | summarize Count=count() by bin(TIMESTAMP, 1m)
  ) on TIMESTAMP
| project Count=iff(isnull(Count), 0, Count), TIMESTAMP
| render timechart  
```
