---
title: parse_urlquery() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 parse_urlquery()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: d8b1b4baecbe3abedf2495fd04d5816a2279fd93
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841266"
---
# <a name="parse_urlquery"></a>parse_urlquery()

返回包含查询参数的 `dynamic` 对象。

**语法**

`parse_urlquery(`*query*`)`

**参数**

* *查询*：字符串表示 url 查询。

**返回**

包含查询参数的[动态](./scalar-data-types/dynamic.md)类型的对象。

**示例**

```kusto
parse_urlquery("k1=v1&k2=v2&k3=v3")
```

将得到以下结果：

```kusto
 {
    "Query Parameters":"{"k1":"v1", "k2":"v2", "k3":"v3"}",
 }
```

**备注**

* 输入格式应遵循 URL 查询标准 (key=value& ...)
 