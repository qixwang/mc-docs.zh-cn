---
title: Set 语句 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 Set 语句。
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
ms.openlocfilehash: 6f0a37bdd2d65eeb4ebcecdd7de641ef6d9cc317
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841716"
---
# <a name="set-statement"></a>Set 语句

::: zone pivot="azuredataexplorer"

`set` 语句用于设置查询持续时间的查询选项。
查询选项控制查询的执行方式并返回结果。 它们可以是布尔标志（默认关闭），也可以是整数值。 一个查询可以包含零个、一个或多个 Set 语句。 Set 语句仅影响按程序顺序尾随这些语句的表格表达式语句。

* 还可以通过在 `ClientRequestProperties` 对象中设置查询选项以编程方式启用查询选项。 [请参阅](../api/netfx/request-properties.md)。
  
* 查询选项并不是 Kusto 语言的正式组成部分，可以修改它，且不会视为中断语言的更改。

**语法**

`set` *OptionName* [`=` *OptionValue*]

**示例**

```kusto
set querytrace;
Events | take 100
```

::: zone-end

::: zone pivot="azuremonitor"

Azure Monitor 不支持此功能

::: zone-end
