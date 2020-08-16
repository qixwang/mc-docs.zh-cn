---
title: url_encode_component() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 url_encode_component()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/17/2020
ms.date: 08/06/2020
ms.openlocfilehash: 8edd313b23c34545084e6fdde326b2230a90ee32
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841545"
---
# <a name="url_encode_component"></a>url_encode_component()

此函数将输入 URL 的字符转换为可通过 Internet 传输的格式。

与 [url_encode](./urlencodefunction.md) 不同的是，它将空格编码为“20%”而不是“+”。

**语法**

`url_encode_component(`url`)`

**参数**

* url：输入 URL（字符串）。  

**返回**

URL（字符串），已转换为可通过 Internet 传输的格式。

**示例**

```kusto
let url = @'https://www.bing.com/hello word/';
print original = url, encoded = url_encode_component(url)
```

|原配|已编码|
|---|---|
|https://www.bing.com/hello word/|https%3a%2f%2f www.bing.com%2fhello%20word|


 