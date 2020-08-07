---
title: array_shift_left() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 array_shift_left()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 08/11/2019
ms.date: 07/31/2020
ms.openlocfilehash: ca0baf6a956fb36b61b2d5ccff94b1879f87ab1d
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509541"
---
# <a name="array_shift_left"></a>array_shift_left()

`array_shift_left()` 将数组中的值向左移动。

## <a name="syntax"></a>语法

`array_shift_left(`*`arr`* , *`shift_count`* `[,` *`fill_value`* ]`)`

## <a name="arguments"></a>参数

* *`arr`* ：要拆分的输入数组，必须是动态数组。
* *`shift_count`* ：整数，用于指定数组元素将向左移动的位置数。 如果该值为负数，则元素将向右移动。
* *`fill_value`* ：用于插入元素的标量值，而不是移动和删除的元素。 默认值：null 值或空字符串（取决于 `arr` 类型）。

## <a name="returns"></a>返回

包含的元素数与原始数组中的元素数相同的动态数组。 每个元素均已根据 shift_count 进行了移动。 为代替已删除元素而添加的新元素的值为 fill_value。

**另请参阅**

* 要向右移动数组，请参阅 [array_shift_right()](array_shift_rightfunction.md)。
* 要向右旋转数组，请参阅 [array_rotate_right()](array_rotate_rightfunction.md)。
* 要向左旋转数组，请参阅 [array_rotate_left()](array_rotate_leftfunction.md)。

## <a name="examples"></a>示例

* 向左移动两个位置：

    <!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
    ```kusto
    print arr=dynamic([1,2,3,4,5]) 
    | extend arr_shift=array_shift_left(arr, 2)
    ```
    
    |`arr`|`arr_shift`|
    |---|---|
    |[1,2,3,4,5]|[3,4,5,null,null]|

* 向左移动两个位置并添加默认值：

    <!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
    ```kusto
    print arr=dynamic([1,2,3,4,5]) 
    | extend arr_shift=array_shift_left(arr, 2, -1)
    ```
    
    |`arr`|`arr_shift`|
    |---|---|
    |[1,2,3,4,5]|[3,4,5,-1,-1]|


* 使用负 shift_count 值向右移动两个位置：

    <!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
    ```kusto
    print arr=dynamic([1,2,3,4,5]) 
    | extend arr_shift=array_shift_left(arr, -2, -1)
    ```
    
    |`arr`|`arr_shift`|
    |---|---|
    |[1,2,3,4,5]|[-1,-1,1,2,3]|
