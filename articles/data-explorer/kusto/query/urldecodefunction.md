---
title: url_decode() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 url_decode()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 23730d2bacd54412e083ca86f5954fbfd3dd1984
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841682"
---
# <a name="url_decode"></a>url_decode()

此函数将编码的 URL 转换为常规 URL 表示形式。 

**语法**

`url_decode(`encoded url`)`

**参数**

* encoded url：编码的 URL（字符串）。  

**返回**

常规表示形式的 URL（字符串）。

**示例**

```kusto
let url = @'https%3a%2f%2fwww.bing.com%2f';
print original = url, decoded = url_decode(url)
```

|原配|已解码|
|---|---|
|https%3a%2f%2f www.bing.com%2f|https://www.bing.com/|



 