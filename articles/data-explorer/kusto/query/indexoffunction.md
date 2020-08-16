---
title: indexof() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 indexof()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 5da963e710dd7fadb0ae94a363f089b215814a94
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841734"
---
# <a name="indexof"></a>indexof()

报告输入字符串中指定的字符串第一次出现时的索引（从零开始）。

如果查找或输入字符串不是字符串类型，此函数会强制将值转换为字符串。

有关详细信息，请参阅 [`indexof_regex()`](indexofregexfunction.md)。

**语法**

`indexof(`*source*`,`*lookup*`[,`*start_index*`[,`*length*`[,`*occurrence*`]]])`

**参数**

* *source*：输入字符串。  
* lookup：要查找的字符串。
* start_index：搜索开始位置。 可选。
* *length*:要检查的字符位置数。 值为 -1 表示长度没有限制。 可选。
* occurrence：出现的次数。 默认值 1。 可选。

**返回**

查找的从零开始的索引位置。

如果在输入中找不到该字符串，则返回 -1。

如果是不相关（小于 0）的 start_index、occurrence 或（小于 -1）length 参数，则返回 null。

**示例**
```kusto
print
 idx1 = indexof("abcdefg","cde")    // lookup found in input string
 , idx2 = indexof("abcdefg","cde",1,4) // lookup found in researched range 
 , idx3 = indexof("abcdefg","cde",1,2) // search starts from index 1, but stops after 2 chars, so full lookup can't be found
 , idx4 = indexof("abcdefg","cde",3,4) // search starts after occurrence of lookup
 , idx5 = indexof("abcdefg","cde",-1)  // invalid input
 , idx6 = indexof(1234567,5,1,4)       // two first parameters were forcibly casted to strings "12345" and "5"
 , idx7 = indexof("abcdefg","cde",2,-1)  // lookup found in input string
 , idx8 = indexof("abcdefgabcdefg", "cde", 1, 10, 2)   // lookup found in input range
 , idx9 = indexof("abcdefgabcdefg", "cde", 1, -1, 3)   // the third occurrence of lookup is not in researched range
```

|idx1|idx2|idx3|idx4|idx5|idx6|idx7|idx8|idx9|
|----|----|----|----|----|----|----|----|----|
|2   |2   |-1  |-1  |    |4   |2   |9   |-1  |
