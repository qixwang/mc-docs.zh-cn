---
title: binary_all_or()（聚合函数）- Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 binary_all_or()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/24/2020
ms.date: 07/31/2020
ms.openlocfilehash: a63459b18e2dd970e4e5d2dffaad9d3ecf22c020
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509229"
---
# <a name="binary_all_or-aggregation-function"></a>binary_all_or()（聚合函数）

使用每个汇总组的二进制 `OR` 操作累计值（如果没有分组就完成了汇总，则返回总计值）。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

## <a name="syntax"></a>语法

summarize `binary_all_or(`*Expr*`)`

## <a name="arguments"></a>参数

* *Expr*：长整型数字。

## <a name="returns"></a>返回

返回使用二进制 `OR` 操作对每个汇总组的记录进行聚合所得的值（如果没有分组就完成了汇总，则返回总计值）。

## <a name="example"></a>示例

使用二进制 `OR` 操作生成“cafe-food”：

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(num:long)
[
  0x88888008,
  0x42000000,
  0x00767000,
  0x00000005, 
]
| summarize result = toupper(tohex(binary_all_or(num)))
```

|result|
|---|
|CAFEF00D|
