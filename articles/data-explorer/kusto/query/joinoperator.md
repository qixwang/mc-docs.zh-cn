---
title: 联接运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的联接运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/30/2020
ms.date: 08/06/2020
zone_pivot_group_filename: data-explorer/zone-pivot-groups.json
zone_pivot_groups: kql-flavors
ms.openlocfilehash: 4d00d60f0f040c0d6334dcb86f1d3e03174568c9
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841313"
---
# <a name="join-operator"></a>join 运算符

通过匹配每个表中指定列的值，合并两个表的行以组成新表。

```kusto
Table1 | join (Table2) on CommonColumn, $left.Col1 == $right.Col2
```

**语法**

*LeftTable* `|` `join` [*JoinParameters*] `(` *RightTable* `)` `on` *Attributes*

**参数**

* LeftTable：要合并其行的**左侧**表或表格表达式（有时称为**外部**表。 表示为 `$left`。

* RightTable：要合并其行的**右侧**表或表格表达式（有时称为**内部*表。 表示为 `$right`。

* Attributes：一个或多个（逗号分隔的）规则，这些规则描述 *LeftTable* 中的行如何与 *RightTable* 中的行进行匹配。 将使用 `and` 逻辑运算符评估多个规则。
  规则可以是下列项之一：

  |规则类型        |语法                                          |Predicate                                                      |
  |-----------------|------------------------------------------------|---------------------------------------------------------------|
  |基于名称的等式 |*ColumnName*                                    |`where` *LeftTable*.*ColumnName* `==` *RightTable*.*ColumnName*|
  |基于值的等式|`$left.`*LeftColumn* `==` `$right.`*RightColumn*|`where` `$left.`*LeftColumn* `==` `$right.`*RightColumn*       |

> [!NOTE]
> 如果使用“基于值的等式”，则列名称必须通过由 `$left` 和 `$right` 表示法表示的相应所有者表进行限定。

* *JoinParameters*：零个或零个以上（以空格分隔）以 Name `=` Value 形式表示的参数，用于控制行匹配操作和执行计划的行为 。 支持以下参数： 

::: zone pivot="azuredataexplorer"

  |名称           |值                                        |说明                                  |
  |---------------|----------------------------------------------|---------------------------------------------|
  |`kind`         |联接风格|请参阅[联接风格](#join-flavors)|                                             |
  |`hint.remote`  |`auto`, `left`, `local`, `right`              |请参阅[跨群集联接](joincrosscluster.md)|
  |`hint.strategy`|执行提示                               |请参阅[联接提示](#join-hints)                |

::: zone-end

::: zone pivot="azuremonitor"

  |名称           |值                                        |说明                                  |
  |---------------|----------------------------------------------|---------------------------------------------|
  |`kind`         |联接风格|请参阅[联接风格](#join-flavors)|                                             |
  |`hint.remote`  |`auto`, `left`, `local`, `right`              |                                             |
  |`hint.strategy`|执行提示                               |请参阅[联接提示](#join-hints)                |

::: zone-end


> [!WARNING]
> 如果未指定 `kind`，则默认的联接风格为 `innerunique`。 这不同于其他的一些分析产品，这些产品以 `inner` 作为默认风格。 请仔细阅读[下文](#join-flavors)，以了解不同种类之间的差异，并确保查询产生预期结果。

**返回**

输出架构取决于联接风格：

 * `kind=leftanti`, `kind=leftsemi`:

     结果表中只包含来自左侧的列。

     
 * `kind=rightanti`, `kind=rightsemi`:

     结果表中只包含来自右侧的列。

     
*  `kind=innerunique`, `kind=inner`, `kind=leftouter`, `kind=rightouter`, `kind=fullouter`

     一列，用于每个表中的每一列，包括匹配键。 如果存在名称冲突，会自动重命名右侧列。

     
输出记录取决于联接风格：

 * `kind=leftanti`, `kind=leftantisemi`

     返回左侧中在右侧没有匹配项的所有记录。     
     
 * `kind=rightanti`, `kind=rightantisemi`

     返回右侧中在左侧没有匹配项的所有记录。  
      
*  `kind=innerunique`, `kind=inner`, `kind=leftouter`, `kind=rightouter`, `kind=fullouter`, `kind=leftsemi`, `kind=rightsemi`

    一行，用于输入表之间的所有匹配项。 匹配项是从一个表中选出的一行，该行中所有 `on` 字段的值与另一个表中某行的对应值相同，具有以下约束：

   - `kind` 未指定，`kind=innerunique`

    左侧中仅有一行与 `on` 键的每个值匹配。 输出包含一行，用于此行与右侧行的每一个匹配项。
    
   - `kind=leftsemi`
   
    返回左侧中在右侧具有匹配项的所有记录。
    
   - `kind=rightsemi`
   
       返回右侧中在左侧具有匹配项的所有记录。

   - `kind=inner`
 
    输出中存在一行，用于左侧和右侧匹配行的每个组合。

   - `kind=leftouter`（或 `kind=rightouter` 或 `kind=fullouter`）

    除了内部匹配，还有一行用于左侧（和/或右侧）的每一行（即使没有匹配项）。 在这种情况下，不匹配的输出单元格包含 null。
    如果这些字段有多个行具有相同的值，则会获得所有合并的行。

 

**提示**

为获得最佳性能：

* 如果某个表始终小于另一个表，则将其用作 join 的左侧（通过管道传送）。

**示例**

从日志中获取扩展活动，某些条目在日志中标记活动的开始和结束时间。 

```kusto
let Events = MyLogTable | where type=="Event" ;
Events
| where Name == "Start"
| project Name, City, ActivityId, StartTime=timestamp
| join (Events
    | where Name == "Stop"
        | project StopTime=timestamp, ActivityId)
    on ActivityId
| project City, ActivityId, StartTime, StopTime, Duration = StopTime - StartTime
```

```kusto
let Events = MyLogTable | where type=="Event" ;
Events
| where Name == "Start"
| project Name, City, ActivityIdLeft = ActivityId, StartTime=timestamp
| join (Events
        | where Name == "Stop"
        | project StopTime=timestamp, ActivityIdRight = ActivityId)
    on $left.ActivityIdLeft == $right.ActivityIdRight
| project City, ActivityId, StartTime, StopTime, Duration = StopTime - StartTime
```

[有关此示例的更多信息](./samples.md#get-sessions-from-start-and-stop-events)。

## <a name="join-flavors"></a>联接风格

联接运算符的确切风格是通过 kind 关键字指定的。 目前，Kusto 支持联接运算符的以下风格： 

|联接种类|描述|
|--|--|
|[`innerunique`](#default-join-flavor)（或默认为空）|执行左侧重复数据删除的内联|
|[`inner`](#inner-join)|标准内联|
|[`leftouter`](#left-outer-join)|左外部联接|
|[`rightouter`](#right-outer-join)|右外部联接|
|[`fullouter`](#full-outer-join)|完全外联|
|[`leftanti`](#left-anti-join)、[`anti`](#left-anti-join) 或 [`leftantisemi`](#left-anti-join)|左反联|
|[`rightanti`](#right-anti-join) 或 [`rightantisemi`](#right-anti-join)|右反联|
|[`leftsemi`](#left-semi-join)|左半联|
|[`rightsemi`](#right-semi-join)|右半联|

### <a name="inner-and-innerunique-join-operator-flavors"></a>inner 和 innerunique 联接运算符风格

-    使用 **inner 联接风格**时，对于在左侧和右侧具有匹配行的每个组合（不删除左侧的重复键），输出中都将存在一行。 输出将是左键和右键的笛卡尔积。
    **内联**示例：

```kusto
let t1 = datatable(key:long, value:string)  
[
1, "val1.1",  
1, "val1.2"  
];
let t2 = datatable(key:long, value:string)  
[  
1, "val1.3", 
1, "val1.4"  
];
t1
| join kind = inner
    t2
on key
```

|key|value|key1|value1|
|---|---|---|---|
|1|val1.2|1|val1.3|
|1|val1.1|1|val1.3|
|1|val1.2|1|val1.4|
|1|val1.1|1|val1.4|

-    使用 **innerunique 联接风格**会删除左侧的重复键，并且对于删除了重复数据的左侧键和右侧键的每个组合，输出中都将存在一行。
    上面使用的相同数据集的 **innerunique 联接**示例。请注意，对于本例，**innerunique 风格**会产生两个可能的输出，两个都是正确的。
    在第一个输出中，联接运算符随机选取 t1 中出现的值为“val1.1”的第一个键，并将其与 t2 键进行匹配，而在第二个输出中，联接运算符随机选取 t1 中出现的值为“val1.2”的第二个键，并将其与 t2 键进行匹配：

```kusto
let t1 = datatable(key:long, value:string)  
[
1, "val1.1",  
1, "val1.2"  
];
let t2 = datatable(key:long, value:string)  
[  
1, "val1.3", 
1, "val1.4"  
];
t1
| join kind = innerunique
    t2
on key
```

|key|value|key1|value1|
|---|---|---|---|
|1|val1.1|1|val1.3|
|1|val1.1|1|val1.4|

```kusto
let t1 = datatable(key:long, value:string)  
[
1, "val1.1",  
1, "val1.2"  
];
let t2 = datatable(key:long, value:string)  
[  
1, "val1.3", 
1, "val1.4"  
];
t1
| join kind = innerunique
    t2
on key
```

|key|value|key1|value1|
|---|---|---|---|
|1|val1.2|1|val1.3|
|1|val1.2|1|val1.4|


-    Kusto 已经过优化，它会尽可能将 `join` 之后的筛选器推向相应的联接左侧或右侧。
    有时候，当使用的风格是 **innerunique** 并且筛选器可以传播到联接的左侧时，它会自动传播，并且应用于该筛选器的键会始终显示在输出中。
    例如，使用上面的示例并添加筛选器 ` where value == "val1.2" ` 会始终生成第二个结果，并且永远不会为使用的数据集生成第一个结果：

```kusto
let t1 = datatable(key:long, value:string)  
[
1, "val1.1",  
1, "val1.2"  
];
let t2 = datatable(key:long, value:string)  
[  
1, "val1.3", 
1, "val1.4"  
];
t1
| join kind = innerunique
    t2
on key
| where value == "val1.2"
```

|key|value|key1|value1|
|---|---|---|---|
|1|val1.2|1|val1.3|
|1|val1.2|1|val1.4|
 
### <a name="default-join-flavor"></a>默认联接风格
    
    X | join Y on Key
    X | join kind=innerunique Y on Key
     
让我们使用两个示例表来说明联接操作： 
 
表 X 

|键 |Value1 
|---|---
|a |1 
|b |2 
|b |3 
|c |4 

表 Y 

|键 |Value2 
|---|---
|b |10 
|c |20 
|c |30 
|d |40 
 
默认联接在左侧对联接键执行重复数据删除后（删除重复数据时会保留第一个记录）执行内联。 对于以下语句： 

    X | join Y on Key 

联接的有效左侧（删除重复数据后的表 X）将是： 

|键 |Value1 
|---|---
|a |1 
|b |2 
|c |4 

联接结果将是： 

```kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join Y on Key
```

|键|Value1|Key1|Value2|
|---|---|---|---|
|b|2|b|10|
|c|4|c|20|
|c|4|c|30|


（注意，键“a”和“d”没有出现在输出中，因为在左侧和右侧都没有匹配的键）。 
 
（历史上，这是 Kusto 的初始版本所支持的联接的第一个实现；这在典型的日志/跟踪分析场景中非常有用。在此类场景中，我们需要将使用同一相关 ID 的两个事件（每个都与某个筛选条件匹配）相关联，获取所有出现的现象（这正是我们要找的），同时忽略多次出现的构成跟踪记录。）
 
### <a name="inner-join"></a>内部联接

这是 SQL 中已知的标准内联。 只要左侧的记录具有与右侧记录相同的联接键，就会生成输出记录。 
 
```kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=inner Y on Key
```

|键|Value1|Key1|Value2|
|---|---|---|---|
|b|3|b|10|
|b|2|b|10|
|c|4|c|20|
|c|4|c|30|

请注意，来自右侧的 (b,10) 联接了两次：与左侧的 (b,2) 和 (b,3) 都进行了联接；此外，左侧的 (c,4) 联接了两次：与右侧的 (c,20) 和 (c,30) 都进行了联接。 

### <a name="left-outer-join"></a>左外部联接 

表 X 和 Y 的左外联的结果始终包含左表 (X) 的所有记录，即使联接条件在右表 (Y) 中未找到任何匹配记录。 
 
```kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=leftouter Y on Key
```

|键|Value1|Key1|Value2|
|---|---|---|---|
|b|3|b|10|
|b|2|b|10|
|c|4|c|20|
|c|4|c|30|
|a|1|||

 
### <a name="right-outer-join"></a>右外部联接 

与左外联类似，但对表的处理是相反的。 
 
```kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=rightouter Y on Key
```

|键|Value1|Key1|Value2|
|---|---|---|---|
|b|3|b|10|
|b|2|b|10|
|c|4|c|20|
|c|4|c|30|
|||d|40|

 
### <a name="full-outer-join"></a>完全外联 

从概念上讲，完全外联合并了应用左外联和右外联的效果。 如果联接的表中的记录不匹配，则对于表中缺少匹配行的每个列，结果集都会有 NULL 值。 对于那些匹配的记录，结果集中会生成单个行（其中包含从两个表中填充的字段）。 
 
```kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=fullouter Y on Key
```

|键|Value1|Key1|Value2|
|---|---|---|---|
|b|3|b|10|
|b|2|b|10|
|c|4|c|20|
|c|4|c|30|
|||d|40|
|a|1|||

 
### <a name="left-anti-join"></a>左反联

左反联返回左侧中在右侧没有任何匹配记录的所有记录。 
 
```kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=leftanti Y on Key
```

|键|Value1|
|---|---|
|a|1|

反联模拟“NOT IN”查询。 

### <a name="right-anti-join"></a>右反联

右反联返回右侧中在左侧没有任何匹配记录的所有记录。 
 
```kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=rightanti Y on Key
```

|键|Value2|
|---|---|
|d|40|

反联模拟“NOT IN”查询。 

### <a name="left-semi-join"></a>左半联

左半联返回左侧中在右侧具有匹配记录的所有记录。 仅会返回左侧的列。 

```kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=leftsemi Y on Key
```

|键|Value1|
|---|---|
|b|3|
|b|2|
|c|4|

### <a name="right-semi-join"></a>右半联

右半联返回右侧中在左侧具有匹配记录的所有记录。 仅会返回右侧的列。 

```kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=rightsemi Y on Key
```

|键|Value2|
|---|---|
|b|10|
|c|20|
|c|30|


### <a name="cross-join"></a>交叉联接

Kusto 本身不支持交叉联接风格（也就是说，你无法使用 `kind=cross` 标记此运算符）。
不过，不难通过虚拟键来模拟这一风格：

    X | extend dummy=1 | join kind=inner (Y | extend dummy=1) on dummy

## <a name="join-hints"></a>联接提示

`join` 运算符支持许多用于控制查询执行方式的提示。
这些提示不会更改 `join` 的语义，但可能会影响其性能。

以下文章解释了联接提示： 
* `hint.shufflekey=<key>` 和 `hint.strategy=shuffle` - [随机执行查询](shufflequery.md)
* `hint.strategy=broadcast` - [广播联接](broadcastjoin.md)
* `hint.remote=<strategy>` - [跨群集联接](joincrosscluster.md)
