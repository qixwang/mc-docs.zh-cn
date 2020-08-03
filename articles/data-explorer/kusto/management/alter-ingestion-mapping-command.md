---
title: .alter ingestion mapping - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .alter ingestion mapping。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: 137f3a75230bfa775a638d2a3058431f41978cc5
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470506"
---
# <a name="alter-ingestion-mapping"></a>.alter ingestion mapping

更改与特定表和特定格式关联的现有引入映射（完全映射替换）。

**语法**

`.alter` `table` TableName `ingestion` MappingKind `mapping` MappingName MappingFormattedAsJson   

> [!NOTE]
> * 此映射可以通过引入命令按其名称引用，而不必将完整映射指定为命令的一部分。
> * MappingKind 的有效值为：`CSV`、`JSON`、`avro`、`parquet` 和 `orc`。

**示例** 
 
```kusto
.alter table MyTable ingestion csv mapping "Mapping1"
'['
'   { "column" : "rownumber", "DataType":"int", "Properties":{"Ordinal":"0"}},'
'   { "column" : "rowguid", "DataType":"string", "Properties":{"Ordinal":"1"} }'
']'

.alter table MyTable ingestion json mapping "Mapping1"
'['
'   { "column" : "rownumber", "Properties":{"Path":"$.rownumber"}},'
'   { "column" : "rowguid", "Properties":{"Path":"$.rowguid"}}'
']'
```

**示例输出**

| 名称     | 种类 | 映射                                                                                                                                                                          |
|----------|------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mapping1 | CSV  | `[{"Name":"rownumber","DataType":"int","CsvDataType":null,"Ordinal":0,"ConstValue":null},{"Name":"rowguid","DataType":"string","CsvDataType":null,"Ordinal":1,"ConstValue":null}]` |
