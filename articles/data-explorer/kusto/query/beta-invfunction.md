---
title: beta_inv() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 beta_inv()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 40eb3a24b57cea1b335e2752a1e39a5b998a57e8
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509522"
---
# <a name="beta_inv"></a>beta_inv()

返回 beta 累积概率 beta 密度函数的反函数。

```kusto
beta_inv(0.1, 10.0, 50.0)
```

若 *probability* = `beta_cdf(`*x*,...`)`，则 `beta_inv(`*probability*,...`)` = *x*。 

在项目计划中，给定预期完成时间和可变性，可以使用 beta 分布对可能的完成时间进行建模。

## <a name="syntax"></a>语法

`beta_inv(`*probability*`, `*alpha*`, `*beta*`)`

## <a name="arguments"></a>参数

* *probability*：与 beta 分布相关的概率。
* *alpha*：分布的一个参数。
* *beta*：分布的一个参数。

## <a name="returns"></a>返回

* beta 累积概率密度函数 [beta_cdf()](./beta-cdffunction.md) 的反函数

**说明**

如果任何参数不是数字，则 beta_inv() 将返回 null 值。

如果 alpha ≤ 0 或 beta ≤ 0，则 beta_inv() 将返回 null 值。

如果 probability ≤ 0 或 probability > 1，beta_inv() 将返回 NaN 值。

给定 probability 值后，beta_inv() 会寻找值 x，使得 beta_cdf(x, alpha, beta) = probability。

## <a name="examples"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(p:double, alpha:double, beta:double, comment:string)
[
    0.1, 10.0, 20.0, "Valid input",
    1.5, 10.0, 20.0, "p > 1, yields null",
    0.1, double(-1.0), 20.0, "alpha is < 0, yields NaN"
]
| extend b = beta_inv(p, alpha, beta)
```

|p|alpha|beta|comment|b|
|---|---|---|---|---|
|0.1|10 个|20 个|有效输入|0.226415022388749|
|1.5|10 个|20 个|p > 1，产生 null||
|0.1|-1|20 个|alpha < 0，产生 NaN|NaN|

**另请参阅**

* 关于计算累积 beta 分布函数，请参阅 [beta-cdf()](./beta-cdffunction.md)。
* 关于计算概率 beta 密度函数，请参阅 [beta-pdf()](./beta-pdffunction.md)。
