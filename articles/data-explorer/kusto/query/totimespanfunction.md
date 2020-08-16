---
title: totimespan() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 totimespan()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 8e37b9f7fd0e99826ead0e56c56beb4cf3dedeef
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841417"
---
# <a name="totimespan"></a>totimespan()

将输入转换为[时间跨度](./scalar-data-types/timespan.md)标量。

```kusto
totimespan("0.00:01:00") == time(1min)
```

**语法**

`totimespan(Expr)`

**参数**

* *`Expr`* ：将转换为[时间跨度](./scalar-data-types/timespan.md)的表达式。

**返回**

如果转换成功，则结果将是[时间跨度](./scalar-data-types/timespan.md)值。
否则，结果将为 null。
