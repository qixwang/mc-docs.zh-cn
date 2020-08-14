---
title: toguid() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 toguid()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: eb466aa17ebbaacaee2ecd9e5b5e3b28bbc0545a
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841696"
---
# <a name="toguid"></a>toguid()

将输入转换为 [`guid`](./scalar-data-types/guid.md) 表现形式。

```kusto
toguid("70fc66f7-8279-44fc-9092-d364d70fce44") == guid("70fc66f7-8279-44fc-9092-d364d70fce44")
```

**语法**

`toguid(`*Expr*`)`

**参数**

* Expr：将转换为 [`guid`](./scalar-data-types/guid.md) 标量的表达式。 

**返回**

如果转换成功，结果将是 [`guid`](./scalar-data-types/guid.md) 标量。
如果转换未成功，结果将是 `null`。

*注意*：如果可能，首选使用 [guid()](./scalar-data-types/guid.md)。