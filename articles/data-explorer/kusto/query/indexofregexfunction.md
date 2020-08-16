---
title: indexof_regex() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 indexof_regex()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 43a2a1e2467ad981a8d844aba5d8e4f5c0840448
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841393"
---
# <a name="indexof_regex"></a>indexof_regex()

函数会报告输入字符串中指定的字符串第一次出现时的索引（从零开始）。 纯字符串匹配项不重叠。

请参阅 [`indexof()`](indexoffunction.md)。

**语法**

`indexof_regex(`*source*`,`*lookup*`[,`*start_index*`[,`*length*`[,`*occurrence*`]]])`

**参数**

|参数     | 描述                                     |必需还是可选|
|--------------|-------------------------------------------------|--------------------|
|source        | 输入字符串                                    |必须            |
|查找        | 要查找的字符串                                  |必须            |
|start_index   | 搜索开始位置                           |可选            |
|length        | 要检查的字符位置数。 -1 定义无限长度 |可选            |
|occurrence    | 查找模式第 N 次出现时的索引。 
                 默认值为 1（第一次出现时的索引） |可选            |

**返回**

查找的从零开始的索引位置。

* 如果在输入中找不到该字符串，则返回 -1。
* 在以下情况下返回 null：
     * start_index 小于 0。
     * 出现次数小于 0。
     * 长度参数小于 -1。


**示例**

```kusto
print
 idx1 = indexof_regex("abcabc", "a.c") // lookup found in input string
 , idx2 = indexof_regex("abcabcdefg", "a.c", 0, 9, 2)  // lookup found in input string
 , idx3 = indexof_regex("abcabc", "a.c", 1, -1, 2)  // there is no second occurrence in the search range
 , idx4 = indexof_regex("ababaa", "a.a", 0, -1, 2)  // Plain string matches do not overlap so full lookup can't be found
 , idx5 = indexof_regex("abcabc", "a|ab", -1)  // invalid input
```

|idx1|idx2|idx3|idx4|idx5|
|----|----|----|----|----|
|0   |3   |-1  |-1  |    |
