---
title: Binary_xor() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 binary_xor()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 369389630bf96717c97d4066169772c1b4aceffd
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509226"
---
# <a name="binary_xor"></a>binary_xor()

返回两个值的位 `xor` 运算的结果。

```kusto
binary_xor(x,y)
```

## <a name="syntax"></a>语法

`binary_xor(`*num1*`,` *num2* `)`

## <a name="arguments"></a>参数

* num1、num2：长整型数字 。

## <a name="returns"></a>返回

返回对一对数字的逻辑 XOR 运算：num1 ^ num2。