---
title: bool 数据类型 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 bool 数据类型。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/31/2020
ms.openlocfilehash: e833812ccc83a3dcd8c4055c19154100ae0a5011
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509241"
---
# <a name="the-bool-data-type"></a>bool 数据类型

`bool` (`boolean`) 数据类型可能具有以下两种状态之一：`true` 或 `false`（内部编码分别为 `1` 和 `0`）以及 NULL 值。

## <a name="bool-literals"></a>bool 文本

`bool` 数据类型具有以下文本：
* `true` 和 `bool(true)`：表示真
* `false` 和 `bool(false)`：表示假
* `bool(null)`：请参阅 [NULL 值](null-values.md)

## <a name="bool-operators"></a>bool 运算符

`bool` 数据类型支持以下运算符：equality (`==`)、inequality (`!=`)、logical-and (`and`) 和 logical-or (`or`)。