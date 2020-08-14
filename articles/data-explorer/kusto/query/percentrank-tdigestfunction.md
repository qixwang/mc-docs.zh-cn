---
title: percentrank_tdigest() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 percentrank_tdigest()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 12/10/2019
ms.date: 08/06/2020
ms.openlocfilehash: 1b4a8b0fd910e153101b40019789c71d62e7ca90
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841607"
---
# <a name="percentrank_tdigest"></a>percentrank_tdigest()

计算集合中值的近似排序，其中排序表示为集大小的百分比。
此函数可以看作是百分位数的倒数。

**语法**

`percentrank_tdigest(`*TDigest*`,` *Expr*`)`

**参数**

* TDigest：[tdigest()](tdigest-aggfunction.md) 或 [tdigest_merge()](tdigest-merge-aggfunction.md) 生成的表达式。
* Expr：表示要用于百分比排序计算的值的表达式。

**返回**

数据集中的值的百分比排序。

**提示**

1) 第二个参数的类型和 `tdigest` 中元素的类型应相同。

2) 第一个参数应是 [tdigest()](tdigest-aggfunction.md) 或 [tdigest_merge()](tdigest-merge-aggfunction.md) 生成的 TDigest

**示例**

获得价值 4490$ 的财产损失的 percentrank_tdigest() 约为 85%：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents
| summarize tdigestRes = tdigest(DamageProperty)
| project percentrank_tdigest(tdigestRes, 4490)

```

|Column1|
|---|
|85.0015237192293|


对财产损失使用百分位数 85 应得出 4490$：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
StormEvents
| summarize tdigestRes = tdigest(DamageProperty)
| project percentile_tdigest(tdigestRes, 85, typeof(long))

```

|percentile_tdigest_tdigestRes|
|---|
|4490|
