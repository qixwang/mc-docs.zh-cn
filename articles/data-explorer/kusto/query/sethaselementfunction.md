---
title: set_has_element() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 set_has_element()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 01/23/2020
ms.date: 08/06/2020
ms.openlocfilehash: ba4669fe2376fb1dbfe1227bd6f18b5950f33885
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841221"
---
# <a name="set_has_element"></a>set_has_element()

确定指定的集中是否包含指定的元素。

**语法**

`set_has_element(`*array*,*value*`)`

**参数**

* *array*：输入要搜索的数组。
* *value*：要搜索的值。 此值的类型应为 `long`、`integer`、`double`、`datetime`、`timespan`、`decimal`、`string` 或 `guid`。

**返回**

True 或 false 取决于数组中是否存在该值。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print arr=dynamic(["this", "is", "an", "example"]) 
| project Result=set_has_element(arr, "example")
```

|结果|
|---|
|1|

**另请参阅**

使用 [`array_index_of(arr, value)`](arrayindexoffunction.md) 查找数组中该值所在的位置。 这两个函数具有相同的性能。
