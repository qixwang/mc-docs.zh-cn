---
title: 行顺序策略 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的行顺序策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 07/01/2020
ms.openlocfilehash: 7f8189026ff203b4f5cdad608af253c670984823
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470422"
---
# <a name="row-order-policy"></a>行顺序策略

行顺序策略是在表上设置的可选策略，它为 Kusto 提供了一个有关数据分片中所需的行顺序的“提示”。

该策略的主要目的不是改善压缩（尽管这是潜在的副作用），而是改善范围已缩小到经过排序的列中一小部分值的查询的性能。

此策略适用于以下情况：

- 大多数查询筛选特定的大维度列的特定值（例如“应用程序 ID”或“租户 ID”）
- 引入表中的数据不太可能按照此列预先排序。

尽管没有对可以定义为策略一部分的列（排序键）数量设置硬编码限制，但是每增加一列都将增加引入过程的开销，并且随着列的增加，有效收益将减少。

> [!NOTE]
> 将策略应用到表后，它将影响自此之后引入的数据。

可在[此处](../management/roworder-policy.md)找到用于管理行顺序策略的控制命令
