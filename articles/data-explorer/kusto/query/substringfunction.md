---
title: substring() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 substring()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: a48311fa2d245b1981c6175b864a212e5fafaedf
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841167"
---
# <a name="substring"></a>substring()

从源字符串的某个索引开始到字符串结尾，提取子字符串。

（可选）可以指定请求子字符串的长度。

```kusto
substring("abcdefg", 1, 2) == "bc"
```

**语法**

`substring(`*source*`,` *startingIndex* [`,` *length*]`)`

**参数**

* *source*：将要从中提取子字符串的源字符串。
* startingIndex：所请求子字符串的从零开始的起始字符位置。
* *length*:一个可选参数，可用于在子字符串中指定所请求的字符数。 

**备注**

startingIndex 可以为负数，在这种情况下，将从源字符串的末尾检索子字符串。

**返回**

给定字符串中的子字符串。 子字符串在 startingIndex（从零开始）字符位置开始，并持续到字符串或长度字符的末尾（如果指定）。

**示例**

```kusto
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
substring("123456", -2, 2)    // 56
```