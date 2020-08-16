---
title: 查询 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的查询。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/06/2020
ms.openlocfilehash: 7bb2d25faf6799a29b775bc720ec5788926263e1
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841187"
---
# <a name="query-operators"></a>查询运算符

查询是针对 Kusto 引擎群集的已引入数据的只读操作。 查询始终在群集中特定数据库的上下文中运行（尽管查询也可以引用其他数据库中的数据，甚至其他群集中的数据）。

由于数据的临时查询是 Kusto 的优先级最高的方案，因此 Kusto 查询语言语法已针对非专家用户进行了优化，用户可以针对其数据创作和运行查询，并且能够清楚地理解（从逻辑上）每个查询的作用。

语言语法是数据流的语法，这里的“数据”实际上表示“表格数据”（一个或多个行/列矩形中的数据）。 查询至少由源数据引用（对 Kusto 表的引用）和依次应用的一个或多个查询运算符组成，通过使用竖线字符 (`|`) 对运算符进行分隔来直观地表示。

例如：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents 
| where State == 'FLORIDA' and StartTime > datetime(2000-01-01)
| count
```
    
以管道字符 `|` 作为前缀的每个筛选器均是运算符** 的一个实例，并且带有某些参数。 运算符的输入是前一管道的结果表。 大多数情况下，任何参数均是输入列上的[标量表达式](./scalar-data-types/index.md)。
在少数情况下，参数是输入列的名称，有时参数是另一个表。 查询结果始终以表呈现，即使它仅有一列和一行。

## <a name="reference-query-operators"></a>参考：查询运算符

> 以下查询示例使用 `T` 表示前一管道或源表。
