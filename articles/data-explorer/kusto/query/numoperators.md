---
title: 数值运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的数值运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 103395a5db28bf8522cf6f43fbca2c9c881e4a6a
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841519"
---
# <a name="numerical-operators"></a>数值运算符

类型 `int`、`long` 和 `real` 表示数值类型。
可以在这些类型对之间使用以下运算符：

运算符       |描述                         |示例
---------------|------------------------------------|-----------------------
`+`            |添加                                 |`3.14 + 3.14`, `ago(5m) + 5m`
`-`            |减                            |`0.23 - 0.22`,
`*`            |相乘                            |`1s * 5`, `2 * 2`
`/`            |除                              |`10m / 1s`, `4 / 2`
`%`            |取模                              |`4 % 2`
`<`            |Less                                |`1 < 10`, `10sec < 1h`, `now() < datetime(2100-01-01)`
`>`            |Greater                             |`0.23 > 0.22`, `10min > 1sec`, `now() > ago(1d)`
`==`           |等于                              |`1 == 1`
`!=`           |不等于                          |`1 != 0`
`<=`           |Less or Equal                       |`4 <= 5`
`>=`           |Greater or Equal                    |`5 >= 4`
`in`           |等于某个元素       |[请参阅此处](inoperator.md)
`!in`          |不等于任何元素   |[请参阅此处](inoperator.md)

**与模数运算符有关的注释**

两个数字的取模始终在 Kusto 中返回一个小的非负数。
因此，两个数字的取模（“N” % “D”）如下 ：0 &le; (*N* % *D*) &lt; abs(*D*).

例如，以下查询：

```kusto
print plusPlus = 14 % 12, minusPlus = -14 % 12, plusMinus = 14 % -12, minusMinus = -14 % -12
```

生成以下结果：

|plusPlus  | minusPlus  | plusMinus  | minusMinus|
|----------|------------|------------|-----------|
|2         | 10         | 2          | 10        |