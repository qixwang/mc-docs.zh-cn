---
title: sequence_detect 插件 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 sequence_detect 插件。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 965a76e589633d500b83bc1701a67e0d745c96a6
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841174"
---
# <a name="sequence_detect-plugin"></a>sequence_detect 插件

根据所提供的谓词检测序列出现次数。

```kusto
T | evaluate sequence_detect(datetime_column, 10m, 1h, e1 = (Col1 == 'Val'), e2 = (Col2 == 'Val2'), Dim1, Dim2)
```

**语法**

*T* `| evaluate` `sequence_detect` `(`*TimelineColumn*`,` *MaxSequenceStepWindow*`,` *MaxSequenceSpan*`,` *Expr1*`,` *Expr2*`,` ..., *Dim1*`,` *Dim2*`,` ...`)`

**参数**

* *T*：输入表格表达式。
* TimelineColumn：表示时间线的列引用必须出现在源表达式中
* MaxSequenceStepWindow：序列中 2 个顺序步骤之间允许的最大时间跨度的标量常数值
* MaxSequenceSpan：序列的最大跨度的标量常数值，用于完成所有步骤
* Expr1, Expr2, ...：定义顺序步骤的布尔谓词表达式
* Dim1, Dim2, ...：用于关联序列的维度表达式

**返回**

返回一个表，其中的每一行都表示出现的单个序列：

* Dim1, Dim2, ...：已用于关联序列的维度列。
* *Expr1*_*TimelineColumn*, *Expr2*_*TimelineColumn*, ...：包含时间值的列，表示每个顺序步骤的时间线。
* 持续时间：序列的整个时间范围

**示例**

### <a name="exploring-storm-events"></a>探索 StormEvents 

以下查询将查看表 StormEvents（2007 年的天气统计），并显示在出现“Excessive Heat”（过热）天气后的 5 天内出现“Wildfire”（野火）的案例。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
StormEvents
| evaluate sequence_detect(
        StartTime,
        5d,  // step max-time
        5d,  // sequence max-time
        heat=(EventType == "Excessive Heat"), 
        wildfire=(EventType == 'Wildfire'), 
        State)
```

|状态|heat_StartTime|wildfire_StartTime|持续时间|
|---|---|---|---|
|加利福尼亚|2007-05-08 00:00:00.0000000|2007-05-08 16:02:00.0000000|16:02:00|
|加利福尼亚|2007-05-08 00:00:00.0000000|2007-05-10 11:30:00.0000000|2.11:30:00|
|加利福尼亚|2007-07-04 09:00:00.0000000|2007-07-05 23:01:00.0000000|1.14:01:00|
|南达科他|2007-07-23 12:00:00.0000000|2007-07-27 09:00:00.0000000|3.21:00:00|
|德克萨斯|2007-08-10 08:00:00.0000000|2007-08-11 13:56:00.0000000|1.05:56:00|
|加利福尼亚|2007-08-31 08:00:00.0000000|2007-09-01 11:28:00.0000000|1.03:28:00|
|加利福尼亚|2007-08-31 08:00:00.0000000|2007-09-02 13:30:00.0000000|2.05:30:00|
|加利福尼亚|2007-09-02 12:00:00.0000000|2007-09-02 13:30:00.0000000|01:30:00|
