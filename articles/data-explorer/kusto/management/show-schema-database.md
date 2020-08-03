---
title: .show 数据库架构 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 .show 数据库架构。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/01/2020
ms.openlocfilehash: dcc5d77573f7d46755a0df23f771e62e1a7158ce
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470448"
---
# <a name="show-databases-schema"></a>.show 数据库架构

返回所选数据库的结构的简单列表，其所有表和列都在单个表或 JSON 对象中。
与版本一起使用时，只有在版本比提供的版本更高时才会返回数据库。

> [!NOTE]
> 该版本只能以“vMM.mm”格式提供。 MM 代表主版本，mm 代表次要版本。

`.show` `database` DatabaseName `schema` [`details`][`if_later_than`“版本”]

`.show` `database` DatabaseName `schema` [`if_later_than`“版本”] `as` `json`

`.show` `databases` `(` DatabaseName1`,` ...`)` `schema` [`details` | `as` `json`]

`.show` `databases` `(` DatabaseName1 if_later_than“版本”`,``)` `schema` [`details` | `as` `json`] 

**示例**

数据库“TestDB”具有 1 个称为“事件”的表。

```kusto
.show database TestDB schema
```

**示例输出**

| DatabaseName | TableName | ColumnName | ColumnType      | IsDefaultTable | IsDefaultColumn | PrettyName | 版本 |
| ------------ | --------- | ---------- | --------------- | -------------- | --------------- | ---------- | ------- |
| TestDB       |           |            |                 | False          | False           |            | v.1.1   |
| TestDB       | 事件    |            |                 | 正确           | False           |            |
| TestDB       | 事件    | 名称       | System.String   | 正确           | False           |            |
| TestDB       | 事件    | StartTime  | System.DateTime | 正确           | False           |            |
| TestDB       | 事件    | EndTime    | System.DateTime | 正确           | False           |            |
| TestDB       | 事件    | 城市       | System.String   | 正确           | False           |            |
| TestDB       | 事件    | SessionId  | System.Int32    | True           | True            |            |

**示例**

```kusto
.show database TestDB schema if_later_than "v1.0"
```

**示例输出**

| DatabaseName | TableName | ColumnName | ColumnType      | IsDefaultTable | IsDefaultColumn | PrettyName | 版本 |
| ------------ | --------- | ---------- | --------------- | -------------- | --------------- | ---------- | ------- |
| TestDB       |           |            |                 | False          | False           |            | v.1.1   |
| TestDB       | 事件    |            |                 | 正确           | False           |            |
| TestDB       | 事件    | 名称       | System.String   | 正确           | False           |            |
| TestDB       | 事件    | StartTime  | System.DateTime | 正确           | False           |            |
| TestDB       | 事件    | EndTime    | System.DateTime | 正确           | False           |            |
| TestDB       | 事件    | 城市       | System.String   | 正确           | False           |            |
| TestDB       | 事件    | SessionId  | System.Int32    | True           | True            |            |

因为提供的版本低于当前数据库的版本，所以返回了“TestDB”架构。 提供相同或更高的版本将生成空结果。

**示例**

```kusto
.show database TestDB schema as json
```

**示例输出**

```json
"{""Databases"":{""TestDB"":{""Name"":""TestDB"",""Tables"":{""Events"":{""Name"":""Events"",""DefaultColumn"":null,""OrderedColumns"":[{""Name"":""Name"",""Type"":""System.String""},{""Name"":""StartTime"",""Type"":""System.DateTime""},{""Name"":""EndTime"",""Type"":""System.DateTime""},{""Name"":""City"",""Type"":""System.String""},{""Name"":""SessionId"",""Type"":""System.Int32""}]}},""PrettyName"":null,""MajorVersion"":1,""MinorVersion"":1,""Functions"":{}}}}"
```
