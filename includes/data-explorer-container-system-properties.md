---
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 02/27/2020
ms.date: 03/16/2020
ms.author: v-tawe
ms.openlocfilehash: aadcb3045db3505dd15412e360ef215d92fcb30e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80243969"
---
### <a name="event-system-properties-mapping"></a>事件系统属性映射

> [!Note]
> * 单记录事件支持系统属性。
> * 对于 `csv` 映射，属性将添加到记录的开头。 对于 `json` 映射，将根据下拉列表中显示的名称添加属性。

如果在表的“数据源”部分中选择了“事件系统属性”，则必须在表架构和映射中包含以下属性。

**表架构示例**

如果数据包含三列（`Timespan`、`Metric` 和 `Value`）并且包含的属性是 `x-opt-enqueued-time` 和 `x-opt-offset`，请使用以下命令创建或更改表架构：

```kusto
    .create-merge table TestTable (TimeStamp: datetime, Metric: string, Value: int, EventHubEnqueuedTime:datetime, EventHubOffset:string)
```

**CSV 映射示例**

运行以下命令，将数据添加到记录的开头。 记下序号值。

```kusto
    .create table TestTable ingestion csv mapping "CsvMapping1"
    '['
    '   { "column" : "Timespan", "Properties":{"Ordinal":"2"}},'
    '   { "column" : "Metric", "Properties":{"Ordinal":"3"}},'
    '   { "column" : "Value", "Properties":{"Ordinal":"4"}},'
    '   { "column" : "EventHubEnqueuedTime", "Properties":{"Ordinal":"0"}},'
    '   { "column" : "EventHubOffset", "Properties":{"Ordinal":"1"}}'
    ']'
```
 
**JSON 映射示例**

使用出现在“数据连接”  边栏选项卡“事件系统属性”  列表中的系统属性名称添加数据。 运行以下命令：

```kusto
    .create table TestTable ingestion json mapping "JsonMapping1"
    '['
    '    { "column" : "Timespan", "Properties":{"Path":"$.timestamp"}},'
    '    { "column" : "Metric", "Properties":{"Path":"$.metric"}},'
    '    { "column" : "Value", "Properties":{"Path":"$.metric_value"}},'
    '    { "column" : "EventHubEnqueuedTime", "Properties":{"Path":"$.x-opt-enqueued-time"}},'
    '    { "column" : "EventHubOffset", "Properties":{"Path":"$.x-opt-offset"}}'
    ']'
```
