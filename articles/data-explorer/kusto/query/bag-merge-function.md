---
title: bag_merge() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 bag_merge()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: elgevork
ms.service: data-explorer
ms.topic: reference
origin.date: 06/18/2020
ms.date: 07/31/2020
ms.openlocfilehash: 9535f6b2ff39ea817e25ef2ad0db7e4eb0b14625
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509527"
---
# <a name="bag_merge"></a>bag_merge()

将 `dynamic` 属性包合并到合并了所有属性的 `dynamic` 属性包中。

## <a name="syntax"></a>语法

`bag_merge(`bag1`, `bag2`[`,` *bag3*, ...])` 

## <a name="arguments"></a>参数

* *bag1...bagN*：输入 `dynamic` 属性包。 该函数接受 2 到 64 个参数。

## <a name="returns"></a>返回

返回 `dynamic` 属性包。 来自合并所有输入属性包对象的结果。 如果一个键出现在多个输入对象中，则将（从此键的可能值中）选择一个任意值。

## <a name="example"></a>示例

表达式：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print result = bag_merge(
   dynamic({'A1':12, 'B1':2, 'C1':3}),
   dynamic({'A2':81, 'B2':82, 'A1':1}))
```

|result|
|---|
|{<br>  "A1":12,<br>  "B1":2,<br>  "C1":3,<br>  "A2":81,<br>  "B2":82<br>}|
