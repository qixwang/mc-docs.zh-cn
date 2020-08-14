---
title: make_string() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 make_string()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 7f1ffb4aaec9f9ee1cb01e216379a32ed4694930
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841648"
---
# <a name="make_string"></a>make_string()

返回由 Unicode 字符生成的字符串。
    
**语法**

`make_string (`*Arg1*[, *ArgN*]... `)`

**参数**

* Arg1 ...ArgN：整数（整形或长整型）表达式或包含整数数组的动态值。

* 此函数最多接收 64 个参数。

**返回**

返回由 Unicode 字符组成的字符串，其码位值由此函数的参数提供。 输入必须包含有效的 Unicode 码位计数。
如有任何参数未映射到 Unicode 字符，该函数将返回 `null`。

**示例**

```kusto
print str = make_string(75, 117, 115, 116, 111)
```

|str|
|---|
|Kusto|

```kusto
print str = make_string(dynamic([75, 117, 115, 116, 111]))
```

|str|
|---|
|Kusto|

```kusto
print str = make_string(dynamic([75, 117, 115]), 116, 111)
```

|str|
|---|
|Kusto|

```kusto
print str = make_string(75, 10, 117, 10, 115, 10, 116, 10, 111)
```

|str|
|---|
|K<br>u<br>秒<br>T<br>o|
