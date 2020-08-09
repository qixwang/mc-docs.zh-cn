---
title: acos() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 acos()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/31/2020
ms.openlocfilehash: cbd975885a0f0bce064bdc1ba7a5a5faf83dc9fc
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509566"
---
# <a name="acos"></a>acos()

返回其余弦为指定数字的角度（[`cos()`](cosfunction.md) 的反运算）。

## <a name="syntax"></a>语法

`acos(`x`)`

## <a name="arguments"></a>参数

* x：范围 [-1, 1] 中的实数。

## <a name="returns"></a>返回

* `x` 的反余弦的值
* `null`（如果 `x` < -1 或者 `x` > 1）