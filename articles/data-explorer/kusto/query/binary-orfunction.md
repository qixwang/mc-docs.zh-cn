---
title: binary_or() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 binary_or()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: ae579f79dc2bdf7b5e3c28f57e55f75b6445bd76
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509230"
---
# <a name="binary_or"></a>binary_or()

返回两个值的位 `or` 运算的结果。 

```kusto
binary_or(x,y)
```

## <a name="syntax"></a>语法

`binary_or(`*num1*`,` *num2* `)`

## <a name="arguments"></a>参数

* num1、num2：长整型数字 。

## <a name="returns"></a>返回

返回对一对数字（num1 | num2）的逻辑 OR 运算。