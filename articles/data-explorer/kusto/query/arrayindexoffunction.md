---
title: Array_index_of() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 array_index_of()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 01/22/2020
ms.date: 07/31/2020
ms.openlocfilehash: 4b17a504bc6bb89b9c368969151102e5a904980b
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509533"
---
# <a name="array_index_of"></a>array_index_of()

搜索指定项的数组，并返回其位置。

## <a name="syntax"></a>语法

`array_index_of(`*array*,*value*`)`

## <a name="arguments"></a>参数

* *array*：输入要搜索的数组。
* *value*：要搜索的值。 此值的类型应为长型、整数型、双精度型、日期/时间型、时间范围型、十进制型、字符串型或 guid。

## <a name="returns"></a>返回

查找的从零开始的索引位置。
如果在数组中找不到该值，则返回 -1。

## <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print arr=dynamic(["this", "is", "an", "example"]) 
| project Result=array_index_of(arr, "example")
```

|结果|
|---|
|3|

**另请参阅**

如果仅希望检查数组中是否存在某个值，但对其位置不感兴趣，可以使用 [set_has_element(`arr`, `value`)](sethaselementfunction.md)。 此函数将提高查询的可读性。 这两个函数具有相同的性能。
