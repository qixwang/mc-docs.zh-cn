---
title: .create ingestion mapping - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .create ingestion mapping。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: bce7163f3aaa3eb313318783a57281d1a5a922a0
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470351"
---
# <a name="create-ingestion-mapping"></a>.create ingestion mapping

创建与特定表和特定格式相关联的引入映射。

**语法**

`.create` `table` TableName `ingestion` MappingKind `mapping` MappingName MappingFormattedAsJson   

> [!NOTE]
>
> - 创建映射后，可在引入命令中按名称引用映射，而不必将完整映射指定为命令的一部分。
> - MappingKind 的有效值为：`CSV`、`JSON`、`avro`、`parquet` 和 `orc`
> - 如果该表已经存在相同名称的映射：
>   - `.create` 将失败
>   - `.create-or-alter` 将更改现有映射

**示例**

```kusto
.create table MyTable ingestion csv mapping "Mapping1"
'['
'   { "column" : "rownumber", "DataType":"int", "Properties":{"Ordinal":"0"}},'
'   { "column" : "rowguid", "DataType":"string", "Properties":{"Ordinal":"1"}}'
']'

.create-or-alter table MyTable ingestion json mapping "Mapping1"
'['
'    { "column" : "rownumber", "datatype" : "int", "Properties":{"Path":"$.rownumber"}},'
'    { "column" : "rowguid", "Properties":{"Path":"$.rowguid"}}'
']'
```

**示例输出**

| 名称     | 种类 | 映射                                                                                                                                                                            |
| -------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mapping1 | CSV  | `[{"Name":"rownumber","DataType":"int","CsvDataType":null,"Ordinal":0,"ConstValue":null},{"Name":"rowguid","DataType":"string","CsvDataType":null,"Ordinal":1,"ConstValue":null}]` |

## <a name="next-steps"></a>后续步骤

有关引入映射的详细信息，请参阅[数据映射](mappings.md)。
