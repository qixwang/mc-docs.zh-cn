---
title: trim_start() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 trim_start()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 5f91cd2b5406b8d467aaae9e7ec09163bee50697
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841478"
---
# <a name="trim_start"></a>trim_start()

删除指定正则表达式的前导匹配项。

**语法**

`trim_start(`*regex*`,` *text*`)`

**参数**

* regex：要从 text 的开头修剪的字符串或[正则表达式](re2.md)。  
* *text*：一个字符串。

**返回**

修剪 text 开头的 regex 匹配项后的 text。

**示例**

以下语句从 string_to_trim 的开头修剪 substring：

```kusto
let string_to_trim = @"https://bing.com";
let substring = "https://";
print string_to_trim = string_to_trim,trimmed_string = trim_start(substring,string_to_trim)
```

|string_to_trim|trimmed_string|
|---|---|
|https://bing.com|bing.com|

下一条语句从字符串的开头修剪所有非单词字符：

```kusto
range x from 1 to 5 step 1
| project str = strcat("-  ","Te st",x,@"// $")
| extend trimmed_str = trim_start(@"[^\w]+",str)
```

|str|trimmed_str|
|---|---|
|-  Te st1// $|Te st1// $|
|-  Te st2// $|Te st2// $|
|-  Te st3// $|Te st3// $|
|-  Te st4// $|Te st4// $|
|-  Te st5// $|Te st5// $|

 