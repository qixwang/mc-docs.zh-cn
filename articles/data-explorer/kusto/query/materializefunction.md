---
title: materialize() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 materialize() 函数。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 06/06/2020
ms.date: 08/06/2020
ms.openlocfilehash: 43a12b85de3b3fcbce230c6f6f7d2c45feac70c6
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841272"
---
# <a name="materialize"></a>materialize()

允许在查询执行期间缓存子查询结果，以便其他子查询可以引用该部分结果。
 
**语法**

`materialize(`*expression*`)`

**参数**

* expression：在执行查询期间要计算和缓存的表格表达式。

**提示**

* 将 materialize 与 join 或 union 一起使用（当其操作数包含可执行一次的相互的子查询时）。 请参阅下面的示例。

* 如果需要联接/联合分支，materialize 也很有用。

* 如果为缓存结果指定了名称，则只能在 let 语句中使用 Materialize。

**注意**

* Materialize 的缓存大小限制为 5 GB。 
  此限制针对每个群集节点，并且对于所有并发运行的查询都是共同的。
  如果查询使用 `materialize()`，而缓存无法保存更多数据，则查询会中止并显示错误。

**示例**

下面的示例演示如何使用 `materialize()` 来提高查询的性能。
表达式 `_detailed_data` 是使用 `materialize()` 函数定义的，因此它只计算一次。

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
let _detailed_data = materialize(StormEvents | summarize Events=count() by State, EventType);
_detailed_data
| summarize TotalStateEvents=sum(Events) by State
| join (_detailed_data) on State
| extend EventPercentage = Events*100.0 / TotalStateEvents
| project State, EventType, EventPercentage, Events
| top 10 by EventPercentage
```

|状态|EventType|EventPercentage|事件|
|---|---|---|---|
|HAWAII WATERS|Waterspout|100|2|
|LAKE ONTARIO|Marine Thunderstorm Wind|100|8|
|GULF OF ALASKA|Waterspout|100|4|
|ATLANTIC NORTH|Marine Thunderstorm Wind|95.2127659574468|179|
|LAKE ERIE|Marine Thunderstorm Wind|92.5925925925926|25|
|E PACIFIC|Waterspout|90|9|
|LAKE MICHIGAN|Marine Thunderstorm Wind|85.1648351648352|155|
|LAKE HURON|Marine Thunderstorm Wind|79.3650793650794|50|
|GULF OF MEXICO|Marine Thunderstorm Wind|71.7504332755633|414|
|HAWAII|High Surf|70.0218818380744|320|


以下示例将生成一组随机数字并计算： 
* 集中有多少不同的值 (Dcount)
* 集中的前三个值 
* 集中所有这些值的总和 
 
此操作可以使用[批处理](batches.md)和 materialize 来完成：

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
let randomSet = 
    materialize(
        range x from 1 to 3000000 step 1
        | project value = rand(10000000));
randomSet | summarize Dcount=dcount(value);
randomSet | top 3 by value;
randomSet | summarize Sum=sum(value)
```

结果集 1：  

|Dcount|
|---|
|2578351|

结果集 2： 

|value|
|---|
|9999998|
|9999998|
|9999997|

结果集 3： 

|Sum|
|---|
|15002960543563|
