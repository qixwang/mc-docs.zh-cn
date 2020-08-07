---
title: binary_shift_left() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 binary_shift_left()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 01c66926375e3c7ca494c77f4d7c41c02c395d99
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509228"
---
# <a name="binary_shift_left"></a>binary_shift_left()

返回针对一对数字的二进制左移运算。

```kusto
binary_shift_left(x,y)  
```

## <a name="syntax"></a>语法

`binary_shift_left(`num1`,` num2 `)` 

## <a name="arguments"></a>参数

* num1、num2：int 数字 。

## <a name="returns"></a>返回

返回针对一对数字的二进制左移运算：num1 << (num2%64)。
如果 n 为负，则返回 NULL 值。