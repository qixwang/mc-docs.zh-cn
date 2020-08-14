---
title: replace() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 replace()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/06/2020
ms.openlocfilehash: 18fd2733df36bd6f8c97a92e3f10b4f822567ca4
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841499"
---
# <a name="replace"></a>replace()

将所有正则表达式匹配项替换为其他字符串。

**语法**

`replace(`regex`,` rewrite`,` text`)`  

**参数**

* regex：用于搜索 text 的[正则表达式](https://github.com/google/re2/wiki/Syntax)。 可在“(括号)”中包含捕获组。 
* rewrite：matchingRegex 执行的任何匹配的替换正则表达式。 使用 `\0` 引用整个匹配项，`\1` 用于第一个捕获组，`\2` 等用于后续捕获组。
* *text*：一个字符串。

**返回**

使用 *rewrite* 计算结果替换 *regex* 的所有匹配项后的 *text*。 匹配项不重叠。

**示例**

此语句：

```kusto
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

具有以下结果：

| x    | str | 替换的内容|
|---|---|---|
| 1    | 数值为 1.000000  | 数值曾为：1.000000|
| 2    | 数值为 2.000000  | 数值曾为：2.000000|
| 3    | 数值为 3.000000  | 数值曾为：3.000000|
| 4    | 数值为 4.000000  | 数值曾为：4.000000|
| 5    | 数值为 5.000000  | 数值曾为：5.000000|
 