---
title: sum()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 sum()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/06/2020
ms.openlocfilehash: cfe0c472c1652d9874a90f4a4e6b365c8dc53342
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841488"
---
# <a name="sum-aggregation-function"></a>sum()（聚合函数）

计算组中 Expr 的总和。 

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

**语法**

summarize `sum(`Expr`)`

**参数**

* Expr：用于聚合计算的表达式。 

**返回**

组中 Expr 的总和值。
 