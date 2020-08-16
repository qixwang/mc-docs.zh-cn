---
title: min_of() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 min_of()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: af63d8de19efc746a99e36e2d0e67a3e1d15dd5a
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841645"
---
# <a name="min_of"></a>min_of()

返回几个计算的数值表达式的最小值。

```kusto
min_of(10, 1, -3, 17) == -3
```

**语法**

`min_of` `(`*expr_1*`,` *expr_2* ...`)`

**参数**

* expr_i：要计算的标量表达式。

- 所有参数的类型必须相同。
- 最多支持 64 个参数。

**返回**

所有参数表达式的最小值。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples  -->
```kusto
print result=min_of(10, 1, -3, 17) 
```

|result|
|---|
|-3|
