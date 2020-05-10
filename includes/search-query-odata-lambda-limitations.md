---
author: brjohnstmsft
ms.service: cognitive-search
ms.topic: include
origin.date: 06/13/2018
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 17e58c80fe4355d7dae39454aabd0e80cde9f14e
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588715"
---
| 数据类型 | 带 `any` 的 lambda 表达式中允许的功能 | 带 `all` 的 lambda 表达式中允许的功能 |
|---|---|---|
| `Collection(Edm.ComplexType)` | 除 `search.ismatch` 和 `search.ismatchscoring` 外的所有内容 | 相同 |
| `Collection(Edm.String)` | 使用 `eq` 或 `search.in` 进行比较 <br/><br/> 使用 `or` 组合子表达式 | 使用 `ne` 或 `not search.in()` 进行比较 <br/><br/> 使用 `and` 组合子表达式 |
| `Collection(Edm.Boolean)` | 使用 `eq` 或 `ne` 进行比较 | 相同 |
| `Collection(Edm.GeographyPoint)` | 将 `geo.distance` 与 `lt` 或 `le` 配合使用 <br/><br/> `geo.intersects` <br/><br/> 使用 `or` 组合子表达式 | 将 `geo.distance` 与 `gt` 或 `ge` 配合使用 <br/><br/> `not geo.intersects(...)` <br/><br/> 使用 `and` 组合子表达式 |
| `Collection(Edm.DateTimeOffset)`、`Collection(Edm.Double)`、`Collection(Edm.Int32)`、`Collection(Edm.Int64)` | 使用 `eq`、`ne`、`lt`、`gt`、`le` 或 `ge` 进行比较 <br/><br/> 使用 `or` 将比较与其他子表达式组合 <br/><br/> 使用 `and` 将除 `ne` 以外的比较与其他子表达式组合 <br/><br/> 在析取范式 (DNF) 中使用 `and` 和 `or` 组合的表达式 | 使用 `eq`、`ne`、`lt`、`gt`、`le` 或 `ge` 进行比较 <br/><br/> 使用 `and` 将比较与其他子表达式组合 <br/><br/> 使用 `or` 将除 `eq` 以外的比较与其他子表达式组合 <br/><br/> 在合取范式 (CNF) 中使用 `and` 和 `or` 组合的表达式 |
