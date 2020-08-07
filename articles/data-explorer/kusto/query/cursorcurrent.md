---
title: cursor_current()、current_cursor() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的 cursor_current()、current_cursor()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 12/10/2019
ms.date: 07/31/2020
zone_pivot_group_filename: data-explorer/zone-pivot-groups.json
zone_pivot_groups: kql-flavors
ms.openlocfilehash: 7d7d7d5b54ea82abb9ae947acb8b14a46bc6f93f
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509504"
---
# <a name="cursor_current-current_cursor"></a>cursor_current()、current_cursor()

::: zone pivot="azuredataexplorer"

检索范围内数据库游标的当前值。 （名称 `cursor_current` 和 `current_cursor` 是同义词。）

## <a name="syntax"></a>语法

`cursor_current()`

## <a name="returns"></a>返回

返回 `string` 类型的单个值，该值对范围内数据库游标的当前值进行编码。

**说明**

有关数据库游标的其他详细信息，请参阅[数据库游标](../management/databasecursor.md)。

::: zone-end

::: zone pivot="azuremonitor"

Azure Monitor 不支持此功能

::: zone-end
