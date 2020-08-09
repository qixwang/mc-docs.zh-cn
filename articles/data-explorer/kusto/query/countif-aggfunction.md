---
title: countif()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 countif()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/31/2020
ms.openlocfilehash: 9721f46eefc23af6b759161e448596839f4ecf30
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509146"
---
# <a name="countif-aggregation-function"></a>countif()（聚合函数）

返回 *Predicate* 对其的计算结果为 `true` 的行数。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

另请参阅 - [count()](count-aggfunction.md) 函数，该函数计算行数（无谓词表达式）。

## <a name="syntax"></a>语法

汇总 `countif(`*Predicate*`)`

## <a name="arguments"></a>参数

* *谓词*：用于聚合计算的表达式。 

## <a name="returns"></a>返回

返回 *Predicate* 对其的计算结果为 `true` 的行数。

> [!TIP]
> 使用 `summarize countif(filter)` 而不是 `where filter | summarize count()`