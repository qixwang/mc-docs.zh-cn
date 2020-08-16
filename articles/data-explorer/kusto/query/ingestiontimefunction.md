---
title: ingestion_time() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 ingestion_time()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 08/06/2020
zone_pivot_group_filename: data-explorer/zone-pivot-groups.json
zone_pivot_groups: kql-flavors
ms.openlocfilehash: fd415068d7dda2abaf4c6d58aa6bafe2625b0464
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841390"
---
# <a name="ingestion_time"></a>ingestion_time()

检索记录的 `$IngestionTime` 隐藏 `datetime` 列（也可能为 null）。
将自动定义 `$IngestionTime` 列（在设置（启用）表的

::: zone pivot="azuredataexplorer"

[IngestionTime 策略](../management/ingestiontimepolicy.md)时）。

::: zone-end

::: zone pivot="azuremonitor"

IngestionTime 策略已设置（已启用）。

::: zone-end

如果表未定义此策略，则返回 null 值。

必须在实际表的上下文中使用此函数以返回相关数据。 例如，如果在 `summarize` 运算符之后调用它，它将为所有记录返回 null。

**语法**

 `ingestion_time()`

**返回**

一个 `datetime` 值，该值指定将数据引入到表中的大致时间。

**示例**

```kusto
T 
| extend ingestionTime = ingestion_time() | top 10 by ingestionTime
```
