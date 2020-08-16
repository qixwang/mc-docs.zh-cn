---
title: strcmp() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 strcmp()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/06/2020
ms.openlocfilehash: aa79f27e5431fe743adecfd79139a92d71156e8b
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841331"
---
# <a name="strcmp"></a>strcmp()

比较两个字符串。

此函数从比较每个字符串的第一个字符开始。 如果第一个字符相同，则继续比较接下来的字符对，直到字符不同或到达较短字符串的末尾。

**语法**

`strcmp(`*string1*`,` *string2*`)` 

**参数**

* string1：要比较的第一个输入字符串。 
* string2：要比较的第二个输入字符串。

**返回**

返回一个整数值，指示字符串之间的关系：
* <0 - 第一个不匹配的字符在 string1 中的值小于 string2 中的值
* 0 - 两个字符串的内容相同
* >0 - 第一个不匹配的字符在 string1 中的值大于 string2 中的值

**示例**

```
datatable(string1:string, string2:string)
["ABC","ABC",
"abc","ABC",
"ABC","abc",
"abcde","abc"]
| extend result = strcmp(string1,string2)
```

|string1|string2|result|
|---|---|---|
|ABC|ABC|0|
|abc|ABC|1|
|ABC|abc|-1|
|abcde|abc|1|