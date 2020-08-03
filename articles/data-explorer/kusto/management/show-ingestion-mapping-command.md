---
title: .show ingestion mappings - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .show ingestion mappings。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/04/2020
ms.date: 07/01/2020
ms.openlocfilehash: 581b65563372f01b57bdddc7b2f6858859fc3e1c
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470456"
---
# <a name="show-ingestion-mappings"></a>.show ingestion mappings

显示引入映射（所有引入映射或按名称指定的某个引入映射）。

- `.show` `table` _TableName_ `ingestion` _MappingKind_ `mappings`

- `.show` `table` _TableName_ `ingestion` _MappingKind_ `mapping` _MappingName_

显示所有映射类型的所有引入映射：

- `.show` `table` _TableName_ `ingestion` `mapping`

**示例**

```kusto
.show table MyTable ingestion csv mapping "Mapping1"

.show table MyTable ingestion csv mappings

.show table MyTable ingestion mappings
```

**示例输出**

| 名称     | 种类 | 映射                                                                                                                                                                            |
| -------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mapping1 | CSV  | `[{"Name":"rownumber","DataType":"int","CsvDataType":null,"Ordinal":0,"ConstValue":null},{"Name":"rowguid","DataType":"string","CsvDataType":null,"Ordinal":1,"ConstValue":null}]` |
