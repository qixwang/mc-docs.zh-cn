---
title: guid 数据类型 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 guid 数据类型。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/15/2020
ms.date: 07/31/2020
ms.openlocfilehash: 250213cdc7995df6374d9ecd0ee764c1586df27b
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509134"
---
# <a name="the-guid-data-type"></a>guid 数据类型

`guid`（`uuid`、`uniqueid`）数据类型表示一个 128 位的全局唯一值。

> [!WARNING]
> 在撰写此文时，对 `guid` 类型的支持尚不完全。
> 主要的不足是此类型的列缺少索引，这会影响其谓词基于该类型的查询的性能。
> 强烈建议相关团队改用类型为 `string` 的值。

## <a name="guid-literals"></a>guid 文本

若要表示 `guid` 类型的文本，请使用以下格式：

```kusto
guid(74be27de-1e4e-49d9-b579-fe0b331d3642)
```

特殊形式 `guid(null)` 表示 [NULL 值](null-values.md)。