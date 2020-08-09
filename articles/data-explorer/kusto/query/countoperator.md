---
title: count 运算符 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 count 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 04/16/2020
ms.date: 07/31/2020
ms.openlocfilehash: 2112eda598c1a65b726fb5bab72e0ce74f784774
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509144"
---
# <a name="count-operator"></a>count 运算符

返回输入记录集中的记录数。

## <a name="syntax"></a>语法

`T | count`

## <a name="arguments"></a>参数

*T*：待计算记录数的表格数据。

## <a name="returns"></a>返回

此函数返回包含单个记录且列类型为 `long` 的表。 唯一单元格的值是 *T* 中的记录数。 

## <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
StormEvents | count
```
