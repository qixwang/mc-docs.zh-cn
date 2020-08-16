---
title: strrep() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 strrep()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 9bb160e83320b29bccb82cc2d805451a54db1369
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841644"
---
# <a name="strrep"></a>strrep()

将给定[字符串](./scalar-data-types/string.md)重复提供的次数。

* 如果第一个或第三个参数不是字符串类型，则会将其强制转换为字符串。

**语法**

`strrep(`*value*,*multiplier*,[*delimiter*]`)`

**参数**

* value：输入表达式
* multiplier：正整数值（从 1 到 1024）
* delimiter：可选字符串表达式（默认值：空字符串）

**返回**

重复指定次数的值，使用 delimiter 进行连接。

如果 multiplier 大于允许的最大值 (1024)，则输入字符串将重复 1024 次。
 
**示例**

```kusto
print from_str = strrep('ABC', 2), from_int = strrep(123,3,'.'), from_time = strrep(3s,2,' ')
```

|from_str|from_int|from_time|
|---|---|---|
|ABCABC|123.123.123|00:00:03 00:00:03|