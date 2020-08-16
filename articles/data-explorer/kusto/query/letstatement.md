---
title: Let 语句 - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的 Let 语句。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: d395fb44eac585a1d35057e5203c5a2c417a971c
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841312"
---
# <a name="let-statement"></a>Let 语句

Let 语句将名称绑定到表达式。 对于作用域中出现了 let 语句的其余部分（全局作用域或在函数正文作用域中），可以使用名称来引用其绑定值。 如果该名称以前绑定到另一个值，则使用“innermost”let 语句绑定。

Let 语句改进了模块化和重用，因为它们允许将可能复杂的表达式分解为多个部分（每个部分都通过 let 语句绑定到一个名称），然后将它们组合在一起。 还可以使用它们创建用户定义的函数和视图（基于表的表达式，其结果看起来像新表）。

Let 语句绑定的名称必须是有效的实体名称。

Let 语句绑定的表达式可以是：
* 标量类型的
* 表格类型的
* 用户定义的函数 (lambda)

**语法**

`let` *Name* `=` *ScalarExpression* | *TabularExpression* | *FunctionDefinitionExpression*

* *名称*：要绑定的名称。 该名称必须是有效的实体名称，并且实体名称转义（例如 `["Name with spaces"]`）是允许的。 
* ScalarExpression：具有标量结果的表达式，其值将绑定到名称。 例如：`let one=1;`。
* TabularExpression：具有表格结果的表达式，其值将绑定到名称。 例如：`Logs | where Timestamp > ago(1h)`。
* FunctionDefinitionExpression：一个表达式，该表达式生成将绑定到名称的 lambda （匿名函数声明）。
  例如：`let f=(a:int, b:string) { strcat(b, ":", a) }`。

Lambda 表达式使用以下语法：

[`view`] `(`[*TabularArguments*][`,`][*ScalarArguments*]`)` `{` *FunctionBody* `}`

`TabularArguments` - [*TabularArgName* `:` `(`[*AtrName* `:` *AtrType*] [`,` ... ]`)`] [`,` ... ][`,`]

 或者：               - [*TabularArgName* `:` `(` `*` `)`]

`ScalarArguments` - [*ArgName* `:` *ArgType*] [`,` ... ]

* `view` 只能出现在无参数 lambda 中（没有任何参数），指示当“所有表”都是查询时（例如，使用 `union *` 时）将包括绑定名称。
* TabularArguments 是正式表格表达式参数的列表。
  每个参数都有：
  * TabularArgName - 正式表格参数的名称。 该名称然后可以出现在 FunctionBody 中，并在调用 lambda 时绑定到特定值。 
  * 表架构定义 - 属性及其类型的列表（AtrName：AtrType）。
  在 lambda 调用中使用的表格表达式必须包含类型匹配的所有这些属性，但不限于它们。 
  “(*)”可用作表格表达式。 在这种情况下，任何表格表达式都可以用在 lambda 调用中，但其列都不能在 lambda 表达式中访问。
  所有表格参数都应位于标量参数之前。
* ScalarArguments 是正式标量参数的列表。 
  每个参数都有：
  * ArgName - 正式标量参数的名称。 该名称然后可以出现在 FunctionBody 中，并在调用 lambda 时绑定到特定值。  
  * ArgType - 正式标量参数的类型。 目前仅支持使用以下类型作为 lambda 参数类型：`bool`、`string`、`long`、`datetime`、`timespan`、`real` 和 `dynamic`（以及这些类型的别名）。

**多个嵌套的 let 语句**

多个 let 语句可以一起使用，以 `;` 作为分隔符，如以下示例所示。
最后一个语句必须是有效的查询表达式： 

```kusto
let start = ago(5h); 
let period = 2h; 
T | where Time > start and Time < start + period | ...
```

允许使用嵌套的 let 语句，并可将其用在 lambda 表达式中。
Let 语句和参数在函数正文的当前作用域和内部作用域中可见。

```kusto
let start_time = ago(5h); 
let end_time = start_time + 2h; 
T | where Time > start_time and Time < end_time | ...
```

**示例**

### <a name="using-let-to-define-constants"></a>使用 let 来定义常量

下面的示例将名称 `x` 绑定到标量文本 `1`，然后在表格表达式语句中使用它：

```kusto
let x = 1;
range y from x to x step x
```

同一个示例，但在本例中，将使用 `['name']` 表示法提供 let 语句的名称：

```kusto
let ['x'] = 1;
range y from x to x step x
```

将 let 用于标量值的另一个示例：

```kusto
let n = 10;  // number
let place = "Dallas";  // string
let cutoff = ago(62d); // datetime
Events 
| where timestamp > cutoff 
    and city == place 
| take n
```

### <a name="using-multiple-let-statements"></a>使用多个 let 语句

下面的示例定义了两个 let 语句，其中一个语句 (`foo2`) 使用另一个语句 (`foo1`)。

```kusto
let foo1 = (_start:long, _end:long, _step:long) { range x from _start to _end step _step};
let foo2 = (_step:long) { foo1(1, 100, _step)};
foo2(2) | count
// Result: 50
```

### <a name="using-materialize-function"></a>使用 materialize 函数

[`materialize`](materializefunction.md) 函数允许在执行查询时缓存子查询结果。 

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
let totalPagesPerDay = PageViews
| summarize by Page, Day = startofday(Timestamp)
| summarize count() by Day;
let materializedScope = PageViews
| summarize by Page, Day = startofday(Timestamp);
let cachedResult = materialize(materializedScope);
cachedResult
| project Page, Day1 = Day
| join kind = inner
(
    cachedResult
    | project Page, Day2 = Day
)
on Page
| where Day2 > Day1
| summarize count() by Day1, Day2
| join kind = inner
    totalPagesPerDay
on $left.Day1 == $right.Day
| project Day1, Day2, Percentage = count_*100.0/count_1


```

|第 1 天|第 2 天|百分比|
|---|---|---|
|2016-05-01 00:00:00.0000000|2016-05-02 00:00:00.0000000|34.0645725975255|
|2016-05-01 00:00:00.0000000|2016-05-03 00:00:00.0000000|16.618368960101|
|2016-05-02 00:00:00.0000000|2016-05-03 00:00:00.0000000|14.6291376489636|
