---
title: parse_url() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 parse_url()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 0c53b038a6ac8ec278adc87cc32ecc5bab077c5f
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841615"
---
# <a name="parse_url"></a>parse_url()

分析绝对 URL `string`，并返回包含 URL 部分的 `dynamic` 对象。


**语法**

`parse_url(`url`)`

**参数**

* url：一个字符串，表示 URL 或 URL 的查询部分。

**返回**

一个[动态](./scalar-data-types/dynamic.md)类型对象，其中包含以下 URL 组件：Scheme、Host、Port、Path、Username、Password、Query Parameters、Fragment。

**示例**

```kusto
T | extend Result = parse_url("scheme://username:password@host:1234/this/is/a/path?k1=v1&k2=v2#fragment")
```

将得到以下结果：

```
 {
    "Scheme":"scheme",
    "Host":"host",
    "Port":"1234",
    "Path":"this/is/a/path",
    "Username":"username",
    "Password":"password",
    "Query Parameters":"{"k1":"v1", "k2":"v2"}",
    "Fragment":"fragment"
 }
```