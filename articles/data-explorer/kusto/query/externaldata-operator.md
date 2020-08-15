---
title: externaldata 运算符 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的外部数据运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/24/2020
ms.date: 08/06/2020
ms.openlocfilehash: 0b07526f2162809732818b748fde3dd77435fcae
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841529"
---
# <a name="externaldata-operator"></a>externaldata 运算符

`externaldata` 运算符返回一个表，该表的架构是在查询自身中定义的，并且该表的数据是从外部存储项目（如 Azure Blob 存储中的 Blob）中读取的。

**语法**

`externaldata` `(` *ColumnName* `:` *ColumnType* [`,` ...] `)` `[` *StorageConnectionString* `]` [`with` `(` *Prop1* `=` *Value1* [`,` ...] `)`]

**参数**

* *ColumnName*, *ColumnType*：这些参数定义表的架构。
  该语法与定义 [.create table](../management/create-table-command.md) 中的表时所使用的语法相同。

* StorageConnectionString：[存储连接字符串](../api/connection-strings/storage.md)描述包含要返回的数据的存储项目。

* *Prop1*, *Value1*, ...：其他属性（如[引入属性](../../ingestion-properties.md)下面所列），描述如何解释从存储中检索到的数据。
    * 当前支持的属性：`format` 和 `ignoreFirstRecord`。
    * 支持的数据格式：支持任何[引入数据格式](../../ingestion-supported-formats.md)，包括 `CSV`、`TSV`、`JSON`、`Parquet`、`Avro`。

> [!NOTE]
> 此运算符没有管道输入。

**返回**

`externaldata` 运算符返回给定架构的数据表，表中的数据是从指定的存储项目中分析的，由存储连接字符串指示。

**示例**

下面的示例显示了如何查找表中的所有记录，该表的 `UserID` 列属于一个已知 ID 集，这些 ID 保存在外部 Blob 中（每行一个 ID）。
由于该集由查询间接引用，因此它可能会很大。

```kusto
Users
| where UserID in ((externaldata (UserID:string) [
    @"https://storageaccount.blob.core.chinacloudapi.cn/storagecontainer/users.txt"
      h@"?...SAS..." // Secret token needed to access the blob
    ]))
| ...
```

下面的示例查询外部存储中存储的多个数据文件。

```kusto
externaldata(Timestamp:datetime, ProductId:string, ProductDescription:string)
[
  h@"https://mycompanystorage.blob.core.chinacloudapi.cn/archivedproducts/2019/01/01/part-00000-7e967c99-cf2b-4dbb-8c53-ce388389470d.csv.gz?...SAS...",
  h@"https://mycompanystorage.blob.core.chinacloudapi.cn/archivedproducts/2019/01/02/part-00000-ba356fa4-f85f-430a-8b5a-afd64f128ca4.csv.gz?...SAS...",
  h@"https://mycompanystorage.blob.core.chinacloudapi.cn/archivedproducts/2019/01/03/part-00000-acb644dc-2fc6-467c-ab80-d1590b23fc31.csv.gz?...SAS..."
]
with(format="csv")
| summarize count() by ProductId
```

可将上述示例视为快速查询多个数据文件（无需定义[外部表](schema-entities/externaltables.md)）的方法。

> [!NOTE]
> `externaldata()` 运算符无法识别数据分区。
