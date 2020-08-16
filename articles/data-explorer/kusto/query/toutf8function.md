---
title: to_utf8() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 to_utf8()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 421d22fb05f8f91c13ad4fb578a023900f15abf3
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841414"
---
# <a name="to_utf8"></a>to_utf8()

返回输入字符串的 unicode 字符的动态数组（make_string 的反运算）。

**语法**

`to_utf8(`*source*`)`

**参数**

* *source*：要转换的源字符串。

**返回**

返回由 unicode 字符组成的动态数组，这些字符组成提供给此函数的字符串。
请参阅 [`make_string()`](makestringfunction.md)。

**示例**

```kusto
print arr = to_utf8("⒦⒰⒮⒯⒪")
```

|arr|
|---|
|[9382, 9392, 9390, 9391, 9386]|

```kusto
print arr = to_utf8("קוסטו - Kusto")
```

|arr|
|---|
|[1511, 1493, 1505, 1496, 1493, 32, 45, 32, 75, 117, 115, 116, 111]|

```kusto
print str = make_string(to_utf8("Kusto"))
```

|str|
|---|
|Kusto|
