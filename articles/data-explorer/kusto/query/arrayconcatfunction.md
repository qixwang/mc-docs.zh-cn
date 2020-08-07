---
title: array_concat() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 array_concat()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/31/2020
ms.openlocfilehash: 5ab7cba87d53f231dc677c391cf7d822a572b47b
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509539"
---
# <a name="array_concat"></a>array_concat()

将多个动态数组连接到单个数组。

## <a name="syntax"></a>语法

`array_concat(`*arr1*`[`,` *arr2*, ...]`)`

## <a name="arguments"></a>参数

* arr1...arrN：要连接到动态数组的输入数组。 所有参数都必须是动态数组（请参阅 [pack_array](packarrayfunction.md)）。 

## <a name="returns"></a>返回

包含 arr1, arr2, ... , arrN 的数组的动态数组。

## <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 3 step 1
| extend y = x * 2
| extend z = y * 2
| extend a1 = pack_array(x,y,z), a2 = pack_array(x, y)
| project array_concat(a1, a2)
```

|Column1|
|---|
|[1,2,4,1,2]|
|[2,4,8,2,4]|
|[3,6,12,3,6]|
