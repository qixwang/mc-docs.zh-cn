---
title: 列 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的列。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 22ddae872eaddc0c4a99913366c2c4a9bad70b55
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509131"
---
# <a name="columns"></a>列

Kusto 中的每个[表](tables.md)和每个表格格式数据流都是一个包含列和行的矩形网格。 表中的每一列都有名称和特定的[标量数据类型](../scalar-data-types/index.md)。 表或表格格式数据流的列是有序的，因此，列在表的列集合中也有特定的位置。

**说明**  

* 列名区分大小写。
* 列名必须遵循[实体名称](./entity-names.md)的规则。
* 每个表的列数的最大限制是 10,000。

在查询中，列通常只按名称引用。 列只能出现在表达式中，而表达式所在的查询运算符决定了表或表格格式数据流，因此不需要进一步限定列名的范围。 例如，在下面的查询中，我们有一个未命名的表格格式数据流（通过包含单个列 (`c`) 的 [datatable 运算符](../datatableoperator.md)来定义）。 然后，将根据该列的值的谓词对表格格式数据流进行筛选，从而生成一个具有相同列但行数较少的新的未命名表格格式数据流。 然后，[as 运算符](../asoperator.md)命名表格格式数据流，其值作为查询结果返回。
请特别注意如何才能按名称引用列 `c` 而不需要引用其容器（实际上，该容器没有名称）：

```kusto
datatable (c:int) [int(-1), 0, 1, 2, 3]
| where c*c >= 2
| as Result
```

> 正如在关系数据库中常见的那样，行有时称为“记录”，列有时称为“属性”。

若要详细了解如何管理列，可参阅[管理列](../../management/columns.md)。