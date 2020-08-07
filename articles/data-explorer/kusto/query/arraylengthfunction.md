---
title: array_length() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 array_length()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: fb94efa04d9c172b5d90ca1a82d48cad637707ea
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509532"
---
# <a name="array_length"></a>array_length()

计算动态数组中的元素数。

## <a name="syntax"></a>语法

`array_length(`array`)`

## <a name="arguments"></a>参数

* array：一个 `dynamic` 值。

## <a name="returns"></a>返回

*array* 中的元素数；如果 *array* 不是数组，则返回`null`。

## <a name="examples"></a>示例

```kusto
print array_length(parse_json('[1, 2, 3, "four"]')) == 4

print array_length(parse_json('[8]')) == 1

print array_length(parse_json('[{}]')) == 1

print array_length(parse_json('[]')) == 0

print array_length(parse_json('{}')) == null

print array_length(parse_json('21')) == null
```