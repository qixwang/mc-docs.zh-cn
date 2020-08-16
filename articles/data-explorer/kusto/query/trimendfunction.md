---
title: trim_end() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 trim_end()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 10d9815de5321e224c439cfa7869619e1d557feb
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841484"
---
# <a name="trim_end"></a>trim_end()

删除指定正则表达式的尾随匹配项。

**语法**

`trim_end(`*regex*`,` *text*`)`

**参数**

* regex：要从 text 的结尾修剪的字符串或[正则表达式](re2.md)。  
* *text*：一个字符串。

**返回**

修剪 text 结尾的 regex 匹配项后的 text。

**示例**

以下语句从 string_to_trim 的结尾修剪 substring：

```kusto
let string_to_trim = @"bing.com";
let substring = ".com";
print string_to_trim = string_to_trim,trimmed_string = trim_end(substring,string_to_trim)
```

|string_to_trim|trimmed_string|
|--------------|--------------|
|bing.com      |bing          |

下一条语句从字符串的结尾修剪所有非单词字符：

```kusto
print str = strcat("-  ","Te st",x,@"// $")
| extend trimmed_str = trim_end(@"[^\w]+",str)
```

|str          |trimmed_str|
|-------------|-----------|
|-  Te st1// $|-  Te st1  |
|-  Te st2// $|-  Te st2  |
|-  Te st3// $|-  Te st3  |
|-  Te st4// $|-  Te st4  |
|-  Te st5// $|-  Te st5  |