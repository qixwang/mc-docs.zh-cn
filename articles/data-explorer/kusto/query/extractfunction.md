---
title: extract() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 extract()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: fe8bfc438cf5a4c78e4656695828dd48a862d9c4
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841403"
---
# <a name="extract"></a>extract()

从文本字符串中获取[正则表达式](./re2.md)的匹配项。 

（可选）将提取的子字符串转换为指明的类型。

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

**语法**

`extract(`*regex*`,` *captureGroup*`,` *text* [`,` *typeLiteral*]`)`

**参数**

* regex：一个[正则表达式](./re2.md)。
* captureGroup：一个正 `int` 常数，指示待提取的捕获组。 0 代表整个匹配项，1 代表正则表达式中第一个“(括号)”匹配的值，2 及以上数字代表后续括号。
* *text*：要搜索的 `string`。
* typeLiteral：可选的类型文本（例如 `typeof(long)`）。 （如果支持）提取的子字符串将转换成此类型。 

**返回**

如果 *regex* 在 *text* 中查找匹配项：与指定捕获组 *captureGroup* 匹配的子字符串可转换为 *typeLiteral*（可选）。

如果没有匹配项，或类型转换失败：`null`。 

**示例**

示例字符串 `Trace` 用于搜索 `Duration` 的定义。 匹配项转换为 `real`，并乘以时间常量 (`1s`)，以便 `Duration` 属于 `timespan` 类型。 在此示例中，此值等于 123.45 秒：

```kusto
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

此示例等效于 `substring(Text, 2, 4)`：

```kusto
extract("^.{2,2}(.{4,4})", 1, Text)
```