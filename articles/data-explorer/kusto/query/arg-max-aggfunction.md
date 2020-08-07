---
title: arg_max()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 arg_max()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/31/2020
ms.openlocfilehash: d25f1969d2a956a89c55eda02f03ebb45cb70c33
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509549"
---
# <a name="arg_max-aggregation-function"></a>arg_max()（聚合函数）

在最大化 ExprToMaximize 的组中查找行，并返回 ExprToReturn 的值（或使用 `*` 返回整个行） 。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

## <a name="syntax"></a>语法

`summarize` [`(`NameExprToMaximize `,` NameExprToReturn [`,` ...] `)=`] `arg_max` `(`ExprToMaximize, `*` | ExprToReturn  [`,` ...]`)`   

## <a name="arguments"></a>参数

* ExprToMaximize：用于聚合计算的表达式。 
* ExprToReturn：当 ExprToMaximize 为最大值时，用于返回值的表达式。 要返回的表达式可以是通配符 (*)，用于返回输入表的所有列。
* NameExprToMaximize：表示 ExprToMaximize 的结果列的可选名称。
* NameExprToReturn：表示 ExprToReturn 的结果列的其他可选名称。

## <a name="returns"></a>返回

在最大化 ExprToMaximize 的组中查找行，并返回 ExprToReturn 的值（或使用 `*` 返回整个行） 。

## <a name="examples"></a>示例

请参阅 [arg_min()](arg-min-aggfunction.md) 聚合函数的示例