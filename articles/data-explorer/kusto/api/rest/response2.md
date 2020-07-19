---
title: 查询 V2 HTTP 响应 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的查询 V2 HTTP 响应。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/11/2020
ms.date: 07/01/2020
ms.openlocfilehash: 2257ba2ef3613bf81a689515c5545af993a6c93c
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226307"
---
# <a name="query-v2-http-response"></a>查询 V2 HTTP 响应

如果状态代码为 200，则响应正文为 JSON 数组。
数组中的每个 JSON 对象称为“帧”。

有多种类型的帧：

* [DataSetHeader](#datasetheader)
* [TableHeader](#tableheader)
* [TableFragment](#tablefragment)
* [TableProgress](#tableprogress)
* [TableCompletion](#tablecompletion)
* [DataTable](#datatable)
* [DataSetCompletion](#datasetcompletion)

## <a name="datasetheader"></a>DataSetHeader 

`DataSetHeader` 帧始终是数据集中的第一个帧，并且只出现一次。

```json
{
    "Version": string,
    "IsProgressive": Boolean
}
```

其中：

* `Version` 是协议版本。 当前版本为 `v2.0`。
* `IsProgressive` 是一个布尔标志，指示此数据集是否包含渐进式帧。 
   渐进式帧是下列项之一：
   
     | Frame             | 说明                                    |
     |-------------------| -----------------------------------------------|
     | `TableHeader`     | 包含有关表的常规信息   |
     | `TableFragment`   | 包含表的矩形数据分片 |
     | `TableProgress`   | 包含以百分比表示的进度 (0-100)       |
     | `TableCompletion` | 指示此帧是最后一帧      |
        
    上述帧描述了一个表。
    如果 `IsProgressive` 标志未设置为 true，则将使用单个帧对集中的每个表进行序列化：
* `DataTable`：包含客户端所需的有关数据集中的单个表的所有信息。

## <a name="tableheader"></a>TableHeader

如果将 `results_progressive_enabled` 选项设置为 true，则进行的查询可能包含此帧。 根据此表，客户端可以期望得到一个包含 `TableFragment` 和 `TableProgress` 帧的交织序列。 表的最后一帧是 `TableCompletion`。

```json
{
    "TableId": Number,
    "TableKind": string,
    "TableName": string,
    "Columns": Array,
}
```

其中：

* `TableId` 是表的唯一 ID。
* `TableKind` 是下列项之一：

    * PrimaryResult
    * QueryCompletionInformation
    * QueryTraceLog
    * QueryPerfLog
    * TableOfContents
    * QueryProperties
    * QueryPlan
    * 未知
      
* `TableName` 是表的名称。
* `Columns` 是描述表的架构的数组。

```json
{
    "ColumnName": string,
    "ColumnType": string,
}
```

[此处](../../query/scalar-data-types/index.md)介绍了支持的列类型。

## <a name="tablefragment"></a>TableFragment

`TableFragment` 帧包含表的矩形数据片段。 除了实际数据之外，此帧还包含一个 `TableFragmentType` 属性，该属性告知客户端应如何处理此片段。 此片段将追加到现有片段，或替换它们。

```json
{
    "TableId": Number,
    "FieldCount": Number,
    "TableFragmentType": string,
    "Rows": Array
}
```

其中：

* `TableId` 是表的唯一 ID。
* `FieldCount` 是表中的列数。
* `TableFragmentType` 描述客户端应如何处理此片段。 
    `TableFragmentType` 是下列项之一：
    
    * DataAppend
    * DataReplace
      
* `Rows` 是包含片段数据的二维数组。

## <a name="tableprogress"></a>TableProgress

`TableProgress` 帧可以与上面所述的 `TableFragment` 帧交织出现。
其唯一用途是告知客户端查询进度。

```json
{
    "TableId": Number,
    "TableProgress": Number,
}
```

其中：

* `TableId` 是表的唯一 ID。
* `TableProgress` 是以百分比表示的进度 (0-100)。

## <a name="tablecompletion"></a>TableCompletion

`TableCompletion` 帧标记表传输的结束。 不会再发送与该表相关的帧。

```json
{
    "TableId": Number,
    "RowCount": Number,
}
```    

其中：

* `TableId` 是表的唯一 ID。
* `RowCount` 是表中的总行数。

## <a name="datatable"></a>DataTable

当 `EnableProgressiveQuery` 标志设置为 false 时，发出的查询将不包括任何帧（`TableHeader`、`TableFragment`、`TableProgress` 和 `TableCompletion`）。 相反，数据集中的每个表都将使用 `DataTable` 帧进行传输，该帧包含客户端读取该表需要的所有信息。

```json
{
    "TableId": Number,
    "TableKind": string,
    "TableName": string,
    "Columns": Array,
    "Rows": Array,
}
```    

其中：

* `TableId` 是表的唯一 ID。
* `TableKind` 是下列项之一：

    * PrimaryResult
    * QueryCompletionInformation
    * QueryTraceLog
    * QueryPerfLog
    * QueryProperties
    * QueryPlan
    * 未知
      
* `TableName` 是表的名称。
* `Columns` 是描述表的架构的数组，并且包括：

```json
{
    "ColumnName": string,
    "ColumnType": string,
}
```

* `Rows` 是包含表的数据的二维数组。

### <a name="the-meaning-of-tables-in-the-response"></a>响应中的表的含义

* `PrimaryResult` - 查询的主要表格结果。 对于每个[表格表达式语句](../../query/tabularexpressionstatements.md)，将按顺序生成一个或多个表，表示该语句产生的结果。 由于存在[批次](../../query/batches.md)和[分支运算符](../../query/forkoperator.md)，可能会有多个这样的表。
* `QueryCompletionInformation` - 提供有关查询本身的执行的附加信息（例如，它是否已成功完成）以及查询使用的资源（类似于 v1 响应中的 QueryStatus 表）。 
* `QueryProperties` - 提供附加值，例如客户端可视化效果说明（发出这些说明是有目的的，例如，反映 [render 运算符](../../query/renderoperator.md)中的信息和[数据库游标](../../management/databasecursor.md)信息）。
* `QueryTraceLog` - 性能跟踪日志信息（当[客户端请求属性](../netfx/request-properties.md)中的 `perftrace` 设置为 true 时返回）。

## <a name="datasetcompletion"></a>DataSetCompletion

`DataSetCompletion` 帧是数据集中的最后一帧。

```json
{
    "HasErrors": Boolean,
    "Cancelled": Boolean,
    "OneApiErrors": Array,
}
```

其中：

* 如果生成数据集时出现错误，则 `HasErrors` 为 true。
* 如果导致生成数据集的请求在完成前被取消，则 `Cancelled` 为 true。 
* 只有 `HasErrors` 为 true 时才会返回 `OneApiErrors`。 有关 `OneApiErrors` 格式的说明，请参阅[此处](https://github.com/Microsoft/api-guidelines/blob/vNext/Guidelines.md)的 7.10.2 部分。
