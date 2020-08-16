---
title: extent_tags() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 extent_tags()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
zone_pivot_group_filename: data-explorer/zone-pivot-groups.json
zone_pivot_groups: kql-flavors
ms.openlocfilehash: d8619f441d8990330be7a44bacfd9e71d7666c3e
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841528"
---
# <a name="extent_tags"></a>extent_tags()

::: zone pivot="azuredataexplorer"

返回一个动态数组，其中包含当前记录所在的数据分片（“盘区”）的[标记](../management/extents-overview.md#extent-tagging)。 

将此函数应用于未附加到数据分片的计算数据会返回空值。

**语法**

`extent_tags()`

**返回**

类型为 `dynamic` 的值，它是包含当前记录的盘区标记的数组，或者是空值。

**示例**

下面的示例演示如何获取一个列表，其中包含其记录是一小时前的所有数据分片的标记，这些记录具有列 `ActivityId` 的特定值。 它表明，一些查询运算符（这里是 `where` 运算符，但对于 `extend` 和 `project` 也是如此）保留了有关承载记录的数据分片的信息。

```kusto
T
| where Timestamp > ago(1h)
| where ActivityId == 'dd0595d4-183e-494e-b88e-54c52fe90e5a'
| extend tags = extent_tags()
| summarize by tostring(tags)
```

下面的示例演示如何获取最近一小时内所有记录的计数，这些记录存储在使用标记 `MyTag`（可能还使用其他标记）标记但没有使用标记 `drop-by:MyOtherTag` 标记的盘区中。

```kusto
T
| where Timestamp > ago(1h)
| extend Tags = extent_tags()
| where Tags has_cs 'MyTag' and Tags !has_cs 'drop-by:MyOtherTag'
| count
```

::: zone-end

::: zone pivot="azuremonitor"

Azure Monitor 不支持此功能

::: zone-end
