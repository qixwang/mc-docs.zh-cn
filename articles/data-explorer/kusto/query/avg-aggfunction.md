---
title: avg()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 avg()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 09/26/2019
ms.date: 07/31/2020
ms.openlocfilehash: d1255aac56650c1c4155df23f958f9d2032bcec5
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509160"
---
# <a name="avg-aggregation-function"></a>avg()（聚合函数）

计算整组中 Expr 的平均值。 

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

## <a name="syntax"></a>语法

summarize `avg(`Expr`)`

## <a name="arguments"></a>参数

* Expr：用于聚合计算的表达式。 具有 `null` 值的记录会被忽略，并且不会包含在计算中。

## <a name="returns"></a>返回

整组中 Expr 的平均值。
 