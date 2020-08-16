---
title: max_of() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 max_of()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: ae8f6a53f7c82d8493fa78fb546ce99307d20c2e
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841368"
---
# <a name="max_of"></a>max_of()

返回几个计算的数值表达式的最大值。

```kusto
max_of(10, 1, -3, 17) == 17
```

**语法**

`max_of` `(`*expr_1*`,` *expr_2* ...`)`

**参数**

* expr_i：要计算的标量表达式。

- 所有参数的类型必须相同。
- 最多支持 64 个参数。

**返回**

所有参数表达式的最大值。

**示例**

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples  -->
```kusto
print result = max_of(10, 1, -3, 17) 
```

|result|
|---|
|17|
