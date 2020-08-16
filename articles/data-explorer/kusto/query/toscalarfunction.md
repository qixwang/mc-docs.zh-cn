---
title: toscalar() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 toscalar()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/06/2020
ms.openlocfilehash: f7e2cbe59feba1338842d739088a5b13a00783bd
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841415"
---
# <a name="toscalar"></a>toscalar()

返回所计算的表达式的标量常数值。 

此函数对于需要暂存计算的查询很有用。 例如，计算事件总数，然后使用结果筛选超过所有事件的特定百分比的组。

**语法**

`toscalar(`*Expression*`)`

**参数**

* *表达式*：将针对标量转换进行计算的表达式。

**返回**

所计算表达式的标量常数值。
如果结果是表格格式，则会采用第一列和第一行进行转换。

> [!TIP]
> 使用 `toscalar()` 时，可以使用 [let 语句](letstatement.md)以提高查询的可读性。

**备注**

可以在查询执行期间对 `toscalar()` 计算常数次数。
`toscalar()` 函数不能应用于行级（for-each-row 方案）。

**示例**

将 `Start`、`End` 和 `Step` 作为标量常数计算，并将结果用于 `range` 计算。

```kusto
let Start = toscalar(print x=1);
let End = toscalar(range x from 1 to 9 step 1 | count);
let Step = toscalar(2);
range z from Start to End step Step | extend start=Start, end=End, step=Step
```

|z|start|end|步骤|
|---|---|---|---|
|1|1|9|2|
|3|1|9|2|
|5|1|9|2|
|7|1|9|2|
|9|1|9|2|
