---
title: trim() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 trim()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: d122239aed66e62164567d860a2a432b21230633
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841480"
---
# <a name="trim"></a>trim()

删除指定正则表达式的所有前导匹配项和尾随匹配项。

**语法**

`trim(`*regex*`,` *text*`)`

**参数**

* regex：要从 text 的开头和/或结尾修剪的字符串或[正则表达式](re2.md)。  
* *text*：一个字符串。

**返回**

修剪 text 开头和/或结尾的 regex 匹配项后的 text。

**示例**

以下语句从 string_to_trim 的开头和结尾修剪 substring：

```kusto
let string_to_trim = @"--https://bing.com--";
let substring = "--";
print string_to_trim = string_to_trim, trimmed_string = trim(substring,string_to_trim)
```

|string_to_trim|trimmed_string|
|---|---|
|--https://bing.com--|https://bing.com|

下一条语句从字符串的开头和结尾修剪所有非单词字符：

```kusto
range x from 1 to 5 step 1
| project str = strcat("-  ","Te st",x,@"// $")
| extend trimmed_str = trim(@"[^\w]+",str)
```

|str|trimmed_str|
|---|---|
|-  Te st1// $|Te st1|
|-  Te st2// $|Te st2|
|-  Te st3// $|Te st3|
|-  Te st4// $|Te st4|
|-  Te st5// $|Te st5|


 