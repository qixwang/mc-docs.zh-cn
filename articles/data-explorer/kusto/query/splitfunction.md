---
title: split() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 split()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 05b72d8b79e5cd3808d6065a64cbeb73397c17f7
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841215"
---
# <a name="split"></a>split()

根据给定分隔符拆分给定字符串，并返回子字符串处于包含状态的字符串数组。

（可选）可以返回特定子字符串（如果存在）。

```kusto
split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]
```

**语法**

`split(`*source*`,` *delimiter* [`,` *requestedIndex*]`)`

**参数**

* *source*：将根据给定分隔符拆分的源字符串。
* *delimiter*：用于拆分源字符串的分隔符。
* *requestedIndex*：可选的从零开始的索引 `int`。 （如果支持）返回的字符串数组将包含请求的子字符串（如果存在）。 

**返回**

包含给定源字符串的子字符串的字符串数组，其中的源字符串使用给定分隔符进行分隔。

**示例**

```kusto
print
    split("aa_bb", "_"),           // ["aa","bb"]
    split("aaa_bbb_ccc", "_", 1),  // ["bbb"]
    split("", "_"),                // [""]
    split("a__b", "_"),            // ["a","","b"]
    split("aabbcc", "bb")          // ["aa","cc"]
```