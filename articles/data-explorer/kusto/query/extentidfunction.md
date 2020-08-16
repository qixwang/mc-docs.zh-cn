---
title: extent_id() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 extent_id()。
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
ms.openlocfilehash: 612400a86c3aa0db7a3760afd66de7fd1c3f04b4
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841534"
---
# <a name="extent_id"></a>extent_id()

::: zone pivot="azuredataexplorer"

返回标识当前记录所在的数据分片（“盘区”）的唯一标识符。 

将此函数应用于未附加到数据分片的计算数据将返回空的 guid（全为零）。

**语法**

`extent_id()`

**返回**

`guid` 类型的值，该值标识当前记录的数据分片或空 guid（全为零）。

**示例**

下面的示例演示如何获取一个列表，其中包含其记录是一小时前的所有数据分片，这些记录具有列 `ActivityId` 的特定值。 它表明，一些查询运算符（这里是 `where` 运算符，但对于 `extend` 和 `project` 也是如此）保留了有关承载记录的数据分片的信息。

```kusto
T
| where Timestamp > ago(1h)
| where ActivityId == 'dd0595d4-183e-494e-b88e-54c52fe90e5a'
| extend eid=extent_id()
| summarize by eid
```

::: zone-end

::: zone pivot="azuremonitor"

Azure Monitor 不支持此功能

::: zone-end
