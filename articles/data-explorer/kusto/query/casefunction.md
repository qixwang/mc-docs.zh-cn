---
title: case() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 case()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/31/2020
ms.openlocfilehash: 3ad1b46230e0afdb67aab9d8211e35779a02d2c8
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509159"
---
# <a name="case"></a>case()

计算谓词列表并返回其谓词满足条件的第一个结果表达式

如果两个谓词都没有返回 `true`，则返回最后一个表达式 (`else`) 的结果。
所有奇数参数（从 1 开始计数）必须是计算结果为 `boolean` 值的表达式。
所有偶数参数 (`then`) 和最后一个参数 (`else`) 的类型必须相同。

## <a name="syntax"></a>语法

`case(`*predicate_1*`,` *then_1*, *predicate_2*`,` *then_2*, *predicate_3*`,` *then_3*, *else*`)`

## <a name="arguments"></a>参数

* *predicate_i*：一个计算结果为 `boolean` 值的表达式。
* *then_i*：一个表达式，在进行计算时，如果 predicate_i 是计算结果为 `true` 的第一个谓词，则从函数返回其值。
* *else*：一个表达式，在进行计算时，如果 predicate_i 的计算结果都不是 `true`，则从函数返回其值。

## <a name="returns"></a>返回

第一个其 predicate_i 的计算结果为 `true` 的 then_i 的值；如果谓词都得不到满足，则为 else 的值。

## <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range Size from 1 to 15 step 2
| extend bucket = case(Size <= 3, "Small", 
                       Size <= 10, "Medium", 
                       "Large")
```

|大小|Bucket|
|---|---|
|1|小型|
|3|小型|
|5|中型|
|7|中型|
|9|中型|
|11|大型|
|13|大型|
|15|大型|
