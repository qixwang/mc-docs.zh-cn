---
title: cursor_before_or_at() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 cursor_before_or_at()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 07/31/2020
zone_pivot_group_filename: data-explorer/zone-pivot-groups.json
zone_pivot_groups: kql-flavors
ms.openlocfilehash: b019640d05792245ff2b20ab906e46a8c90b3aff
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509242"
---
# <a name="cursor_before_or_at"></a>cursor_before_or_at()

::: zone pivot="azuredataexplorer"

针对表记录的谓词，用于将记录的引入时间与数据库游标值进行比较。

## <a name="syntax"></a>语法

`cursor_before_or_at` `(` RHS `)`

## <a name="arguments"></a>参数

* RHS：空字符串文本或有效的数据库游标值。

## <a name="returns"></a>返回

`bool` 类型的标量值，指示记录的引入时间是否在数据库游标 RHS 之前或之时 (`true`) 或非 (`false`)。

**说明**

有关数据库游标的其他详细信息，请参阅[数据库游标](../management/databasecursor.md)。

只能对已启用 [IngestionTime 策略](../management/ingestiontimepolicy.md)的表记录调用此函数。

::: zone-end

::: zone pivot="azuremonitor"

Azure Monitor 不支持此功能

::: zone-end
