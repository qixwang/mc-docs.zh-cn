---
title: array_slice() - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的 array_slice()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 12/03/2018
ms.date: 07/31/2020
ms.openlocfilehash: 3ddd884f65adbc6cb120f09b3317b766fd5301b2
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509255"
---
# <a name="array_slice"></a>array_slice()

提取动态数组的切片。

## <a name="syntax"></a>语法

`array_slice`( *`arr`* , *`start`* , *`end`* )

## <a name="arguments"></a>参数

* *`arr`* ：要从中提取切片的输入数组必须是动态数组。
* `start`：切片的从零开始（包括零）的开始索引，负值转换为 array_length+start。
* `end`：切片的从零开始（包括零）的结束索引，负值转换为 array_length+end。

注意：忽略超出范围的索引。

## <a name="returns"></a>返回

`arr` 中处于 [`start..end`] 范围内的值的动态数组。

## <a name="examples"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print arr=dynamic([1,2,3]) 
| extend sliced=array_slice(arr, 1, 2)
```
|`arr`|`sliced`|
|---|---|
|[1,2,3]|[2,3]|

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print arr=dynamic([1,2,3,4,5]) 
| extend sliced=array_slice(arr, 2, -1)
```
|`arr`|已切片|
|---|---|
|[1,2,3,4,5]|[3,4,5]|

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print arr=dynamic([1,2,3,4,5]) 
| extend sliced=array_slice(arr, -3, -2)
```
|`arr`|已切片|
|---|---|
|[1,2,3,4,5]|[3,4]|
